<template>
  <div>
    <h1>Simple Table</h1>
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Surname</th>
          <th>Birthday</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="row in rows" :key="row.id">
          <td>{{ row.id }}</td>
          <td>{{ row.name }}</td>
          <td>{{ row.surname }}</td>
          <td>{{ row.birthday }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      rows: []
    };
  },
  created() {
    
    for (let i = 1; i <= 20; i++) {
      this.rows.push({
        id: i,
        name: `Name ${i}`,
        surname: `Surname ${i}`,
        birthday: `1990-01-${i < 10 ? '0' + i : i}`
      });
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
  background-color: #322d2d;
}
</style>