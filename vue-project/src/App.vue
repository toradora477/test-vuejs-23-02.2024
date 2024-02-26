<template>
  <div>
    <h1>Simple Table</h1>

    <div>
      <input v-model="newRecord.id" placeholder="ID">
      <input v-model="newRecord.name" placeholder="Name">
      <input v-model="newRecord.surname" placeholder="Surname">
      <input v-model="newRecord.birthday" placeholder="Birthday">
      <button @click="addRecord">Add</button>
    </div>

    <table>
      <thead>
        <tr>
          <th @click="sortBy('id')">ID <button @click="changeSortDirection('id')">Sort</button></th>
          <th @click="sortBy('name')">Name <button @click="changeSortDirection('name')">Sort</button></th>
          <th @click="sortBy('surname')">Surname <button @click="changeSortDirection('surname')">Sort</button></th>
          <th @click="sortBy('birthday')">Birthday <button @click="changeSortDirection('birthday')">Sort</button></th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, index) in paginatedRows" :key="row.id">
          <td @dblclick="editCell(row.id, 'id')">
            <span v-if="editingCell === row.id && editingColumn === 'id'">
              <input v-model="row.id" @blur="saveCell(row.id, 'id')" @keyup.enter="saveCell(row.id, 'id')">
            </span>
            <span v-else>{{ row.id }}</span>
          </td>
          <td @dblclick="editCell(row.id, 'name')">
            <span v-if="editingCell === row.id && editingColumn === 'name'">
              <input v-model="row.name" @blur="saveCell(row.id, 'name')" @keyup.enter="saveCell(row.id, 'name')">
            </span>
            <span v-else>{{ row.name }}</span>
          </td>
          <td @dblclick="editCell(row.id, 'surname')">
            <span v-if="editingCell === row.id && editingColumn === 'surname'">
              <input v-model="row.surname" @blur="saveCell(row.id, 'surname')" @keyup.enter="saveCell(row.id, 'surname')">
            </span>
            <span v-else>{{ row.surname }}</span>
          </td>
          <td @dblclick="editCell(row.id, 'birthday')">
            <span v-if="editingCell === row.id && editingColumn === 'birthday'">
              <input v-model="row.birthday" @blur="saveCell(row.id, 'birthday')" @keyup.enter="saveCell(row.id, 'birthday')">
            </span>
            <span v-else>{{ row.birthday }}</span>
          </td>
          <td>
            <button @click="deleteRecord(row.id)">Delete</button>
          </td>
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
      newRecord: {
        id: '',
        name: '',
        surname: '',
        birthday: ''
      },
      pageSize: 5,
      currentPage: 1,
      sortByColumn: null,
      sortDirection: 'asc',
      searchQuery: '',
      editingCell: null,
      editingColumn: null
    };
  },
  computed: {
    sortedRows() {
      let filteredRows = this.rows;
      if (this.searchQuery) {
        const query = this.searchQuery.toLowerCase();
        filteredRows = this.rows.filter(row => {
          return Object.values(row).some(value => {
            return value.toString().toLowerCase().includes(query);
          });
        });
      }

      if (!this.sortByColumn) return filteredRows;
      return filteredRows.slice().sort((a, b) => {
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
    for (let i = 1; i <= 20; i++) {
      this.rows.push({
        id: i,
        name: `Name ${i}`,
        surname: `Surname ${i}`,
        birthday: `${i < 10 ? '0' + i : i}-01-1995`
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

      this.sortedRows.sort((a, b) => {
      const valueA = String(a[column]).toLowerCase();
      const valueB = String(b[column]).toLowerCase();
      if (this.sortDirection === 'asc') {
        return valueA.localeCompare(valueB);
      } else {
        return valueB.localeCompare(valueA);
      }
  });
    },
    changeSortDirection(column) {
      if (this.sortByColumn === column) {
        this.sortDirection = this.sortDirection === 'asc' ? 'desc' : 'asc';
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
    },
    editCell(id, column) {
      this.editingCell = id;
      this.editingColumn = column;
    },
    saveCell(id, column) {
      this.editingCell = null;
      this.editingColumn = null;
    },
    deleteRecord(id) {
      const index = this.rows.findIndex(row => row.id === id);
      if (index !== -1) {
        this.rows.splice(index, 1);
      }
    },
    addRecord() {
      this.rows.unshift({ ...this.newRecord });
      this.newRecord = {
        id: '',
        name: '',
        surname: '',
        birthday: ''
      };
    }
  }
};
</script>

<style>

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid black;
  padding: 8px;
}

th {
  background-color: #272626;
}
</style>
