<template>
  <div id="userlist-wrapper">
        <table class="table table-striped text-center">
            <thead>
                <tr class="table-primary">
                    <th scope="col">ID</th>
                    <th scope="col">Name</th>
                    <th scope="col">Email</th>
                    <th scope="col">Action</th>
                </tr>
            </thead>
            <tbody>
                <slot>Default</slot>
            </tbody>
        </table>
  </div>
</template>

<script>
export default {
    name: 'UserList'
}
</script>
