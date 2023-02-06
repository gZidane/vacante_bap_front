<template>
    <div class="container">
        <div class="taskContainer" v-if="task != null">
            <h2>Task details</h2>
            <h3>{{ task.title }}</h3>

            <p>Assigned to:<br> {{ task.assigned_to }}</p>
            <p>Delivery date:<br> {{ task.delivery_date }}</p>
            <p>Description:<br> {{ task.description }}</p>
            <p>Comments:<br> {{ task.comments }}</p>
            <p>Status:<br> {{ task.status }}</p>

            <div class="btnsContainer">
                <button v-on:click="deleteTask()">Delete task</button>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'IndexPage',

  data() {
      return {
          headers: { Authorization: "Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd" },
          task: null
      }

  },
  methods:
  {
      getPostById: function(id)
      {
          return new Promise((resolve, reject) =>
          {
            // fetch("https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/" + id,
            // {
            //     headers: { Authorization: "Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd" }
            // })
            fetch("http://localhost:4000/task/getById?id=" + id)
              .then(response =>
              {
                // manejar el parametro status de la respuesta
                response.json().then(result =>
                {
                    resolve(result);
                });

              })
              .catch(err =>
              {
                console.log(err);

              });
        });

      },
      deleteTask: function(id)
      {
          return new Promise((resolve, reject) =>
          {
            // fetch("https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/" + id,
            // {
            //     headers: { Authorization: "Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd" }
            // })
            fetch("http://localhost:4000/task/delete", { method: 'delete' })
              .then(response =>
              {
                // manejar el parametro status de la respuesta
                response.json().then(result =>
                {
                    resolve(result);
                });

              })
              .catch(err =>
              {
                console.log(err);

              });
        });

      },

  },
  created()
  {

      this.getPostById(this.$route.params.id)
      .then(response =>
      {

        console.log(response.data[0]);
        this.task = response.data[0];
        this.$forceUpdate();

      })
      .catch(err =>
      {
        console.log(err);

      });
  },
})
</script>

<style scoped>
.container
{
    width: 100%;
    height: auto;
    min-height: 100%;
    top: 0;
    left: 0;
    background: rgb(94,39,251);
    background: linear-gradient(180deg, rgba(94,39,251,1) 0%, rgba(255,4,87,0.16708681763721112) 100%);
    padding: 50px 0;
    padding-bottom: 75px;
    box-sizing: border-box;
    position: absolute;
}
.taskContainer
{
    width: 600px;
    margin: 0 auto;
    border-radius: 20px;
    background: #fff;
    margin: 0 auto;
    padding: 30px 10px;
}
@media (max-width: 577px)
{
    .taskContainer
    {
        width: 90%;
    }
}
</style>
