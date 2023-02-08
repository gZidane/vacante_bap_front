<template>
    <div class="container">
        <div class="taskContainer" v-if="task != null">
            <h2>Task details</h2>
            <div class="taskImgCont">
                <img src="@/static/TASK2.webp" />
            </div>

            <h3>{{ task.title }}</h3>

            <p>Assigned to:<br> {{ task.assigned_to }}</p>
            <p>Delivery date:<br> {{ task.delivery_date }}</p>
            <p>Description:<br> {{ task.description }}</p>
            <p>Comments:<br> {{ task.comments }}</p>
            <p>Status:<br> {{ task.status }}</p>

            <div class="btnsContainer">
                <button v-on:click="showModalFunction()">Delete task</button>
            </div>

        </div>

        <div class="modalBackground" v-if="showModal == true">
            <div class="deleteConfirmModal">
                <p>Are you sure you want to delete this task?</p>
                <div class="confirmBtnsCont">
                    <button v-on:click="hideModal()">Cancel</button>
                    <button v-on:click="deleteTask()">Yes, delete</button>
                </div>
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
          task: null,
          showModal: false
      }

  },
  methods:
  {
      showModalFunction: function()
      {
          this.showModal = true;

          console.log(this.showModal);
      },
      hideModal: function()
      {
          this.showModal = false;
          this.$forceUpdate();

          console.log(this.showModal);
      },
      getPostById: function(id: string)
      {
          return new Promise((resolve, reject) =>
          {
            // fetch("https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/" + id,
            // {
            //     headers: { Authorization: "Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd" }
            // })
            fetch("http://localhost:4000/task/getById?id=" + id)
              .then((resp) =>
              {
                // manejar el parametro status de la respuesta
                resp.json().then(result =>
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
      deleteTask: function()
      {

          let id = this.$route.params.id;

          return new Promise((resolve, reject) =>
          {
            // fetch("https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/" + id,
            // {
            //     headers: { Authorization: "Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd" }
            // })
            fetch("http://localhost:4000/task/delete/" + id, { method: 'delete' })
              .then((resp) =>
              {
                // manejar el parametro status de la respuesta
                resp.json().then(result =>
                {
                    alert("Task deleted succesfully");
                    location.href = '../';
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
      .then((resp) =>
      {

        this.task = (resp as any).data[0];
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
@keyframes fadeUp
{
    0%
    {
        opacity: 0;
        transform: translateY(30px);
    }
    100%
    {
        opacity: 1;
        transform: none;
    }

}
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
    animation: fadeUp 1s ease-in-out;
}
.taskContainer h2
{
    margin: 0;
    text-align: center;
}
.taskImgCont
{
    width: 100%;
    text-align: center;
}
.taskImgCont img
{
    width: 65%;
}
.btnsContainer
{
    width: 100%;
    height: auto;
    text-align: center;
}
.btnsContainer button
{
    width: 80%;
    height: 35px;
    font-size: 16px;
    border: none;
    border-radius: 5px;
    color: #C70039;
    cursor: pointer;
}
.modalBackground
{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.7);
    display: flex;
    justify-content: center;
    align-items: center;
}
.deleteConfirmModal
{
    width: 500px;
    height: auto;
    padding: 30px 20px;
    box-sizing: border-box;
    background: #fff;
    box-shadow: 5px 15px 15px rgba(0,0,0,0.4);
    text-align: center;
    border-radius: 5px;
}
.confirmBtnsCont
{
    width: 100%;
    text-align: center;
    margin-top: 30px;
}
.confirmBtnsCont button
{
    padding: 0 25px;
    height: 35px;
    margin-right: 10px;
    border: none;
    border-radius: 5px;
    background: #C70039;
    color: #fff;
    cursor: pointer;
}
.confirmBtnsCont button:nth-child(1)
{
    background: #e5e5e5;
    color: inherit;
}
@media (max-width: 577px)
{
    .taskContainer
    {
        width: 90%;
    }
    .taskImgCont
    {
        text-align: center;
    }
    .taskImgCont img
    {
        width: 90%;
    }
    .deleteConfirmModal
    {
        width: 90%;
    }
}
</style>
