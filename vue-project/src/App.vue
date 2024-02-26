<template>
  <div>
    <h1>Simple Table</h1>
    <table>
      <thead>
        <tr>
          <th @click="sortBy('id')">ID <button @click="changeSortDirection('id')">Sort</button></th>
          <th @click="sortBy('name')">Name <button @click="changeSortDirection('name')">Sort</button></th>
          <th @click="sortBy('surname')">Surname <button @click="changeSortDirection('surname')">Sort</button></th>
          <th @click="sortBy('birthday')">Birthday <button @click="changeSortDirection('birthday')">Sort</button></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, index) in paginatedRows" :key="row.id">
          <td>{{ row.id }}</td>
          <td>{{ row.name }}</td>
          <td>{{ row.surname }}</td>
          <td>{{ row.birthday }}</td>
        </tr>
      </tbody>
    </table>
    <div>
      <button @click="previousPage" :disabled="currentPage === 1">Previous</button>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      rows: [],
      pageSize: 5,
      currentPage: 1,
      sortByColumn: null,
      sortDirection: 'asc'
    };
  },
  computed: {
    sortedRows() {
      if (!this.sortByColumn) return this.rows;
      return this.rows.slice().sort((a, b) => {
        const valueA = a[this.sortByColumn];
        const valueB = b[this.sortByColumn];
        
        if (this.sortDirection === 'asc') {
          return valueA.localeCompare(valueB);
        } else {
          return valueB.localeCompare(valueA);
        }
      });
    },
    paginatedRows() {
      const startIndex = (this.currentPage - 1) * this.pageSize;
      return this.sortedRows.slice(startIndex, startIndex + this.pageSize);
    },
    totalPages() {
      return Math.ceil(this.sortedRows.length / this.pageSize);
    }
  },
  created() {
    // Генерируем 20 дефолтных записей
    for (let i = 1; i <= 20; i++) {
      this.rows.push({
        id: i,
        name: `Name ${i}`,
        surname: `Surname ${i}`,
        birthday: `1990-01-${i < 10 ? '0' + i : i}`
      });
    }
  },
  methods: {
    sortBy(column) {
      if (this.sortByColumn === column) {
        this.sortDirection = this.sortDirection === 'asc' ? 'desc' : 'asc';
      } else {
        this.sortByColumn = column;
        this.sortDirection = 'asc';
      }
    },
    changeSortDirection(column) {
      if (this.sortByColumn === column) {
        this.sortDirection = this.sortDirection === 'asc' ? 'desc' : 'asc';
      } else {
        this.sortByColumn = column;
        this.sortDirection = 'asc';
      }
    },
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    }
  }
};
</script>

<style>
/* Можете добавить стилизацию таблицы здесь, если необходимо */
table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid black;
  padding: 8px;
}

th {
  background-color: #131111;
}
</style>
