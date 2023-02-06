<template>
    <div>
        <div v-if="task != null">
            <h3>{{ task.title }}</h3>
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

</style>
