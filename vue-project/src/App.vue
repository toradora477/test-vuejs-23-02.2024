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
        <tr>
          <td>1</td>
          <td>John</td>
          <td>Doe</td>
          <td>1990-01-01</td>
        </tr>
        <tr>
          <td>2</td>
          <td>Jane</td>
          <td>Smith</td>
          <td>1995-05-15</td>
        </tr>
        <!-- Добавьте больше строк, если необходимо -->
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'App',
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
  background-color: #f2f2f2;
}
</style>
