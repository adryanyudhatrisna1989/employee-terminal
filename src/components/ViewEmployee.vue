<template>
    <div id="view-employee">
        <ul class="collection with-header">
            <li class="collection-header"><h4>{{name}}</h4></li>
            <li class="collection-item">Developer ID: {{employee_id}}</li>
            <li class="collection-item">Department: {{dept}}</li>
            <li class="collection-item">Postion: {{position}}</li>
        </ul>
        <router-link to="/" class="btn gray">Back</router-link>
        <button @click="deleteEmployee" class="btn pink">Delete</button>

        <div class="fixed-action-btn">
            <router-link v-bind:to="{ name: 'edit-employee', params: { employee_id: employee_id }}" class="btn-floating btn-normal pink">
                <i class="fa fa-pencil-square-o"></i>
            </router-link>
        </div>
    </div>
</template>
    </div>
</template>

<script>
import db from './firebaseInit';
export default {
  name: 'view-employee',
  data() {
    return {
      employee_id: null,
      name: null,
      dept: null,
      position: null
    };
  },
  beforeRouteEnter(to, from, next) {
    db
      .collection('employees')
      .where('employee_id', '==', to.params.employee_id)
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          next(vm => {
            vm.employee_id = doc.data().employee_id;
            vm.name = doc.data().name;
            vm.dept = doc.data().dept;
            vm.position = doc.data().position;
          });
        });
      });
  },
  watch: {
    $route: 'fetchData'
  },
  methods: {
    fetchData() {
      db
        .collection('employees')
        .where('employee_id', '==', this.$route.params.employee_id)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            this.employee_id = doc.data().employee_id;
            this.name = doc.data().name;
            this.dept = doc.data().dept;
            this.position = doc.data().position;
          });
        });
    },
    deleteEmployee() {
      if (confirm('Are you sure?')) {
        db
          .collection('employees')
          .where('employee_id', '==', this.$route.params.employee_id)
          .get()
          .then(querySnapshot => {
            querySnapshot.forEach(doc => {
              doc.ref.delete();
              this.$router.push('/');
            });
          });
      }
    }
  }
};
</script>