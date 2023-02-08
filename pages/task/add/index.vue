<template>
    <div class="container">
        <div class="addTaskCont">
            <h3>Add new task</h3>
            <img src="" />

                <input id="t_title" type="text" placeholder="Task title" />
                <input id="t_desc" type="text" placeholder="Task description" />
                <input id="t_status" type="text" placeholder="status" />
                <input id="t_assigned" type="text" placeholder="Assigned to" />
                <input id="t_dd" type="text" placeholder="Delivery date" />
                <textarea id="t_comments" type="text" placeholder="Comments" /></textarea>

                <button v-on:click="addNewTask()">Add task</button>
        </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'AddNewTask',

  data() {
      return {
          headers: { Authorization: "Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd" }
      }

  },
  methods:
  {
      addNewTask: function()
      {

          let t_title = (document.getElementById("t_title") as HTMLInputElement).value;
          let t_desc = (document.getElementById("t_desc") as HTMLInputElement).value;
          let t_status = (document.getElementById("t_status") as HTMLInputElement).value;
          let t_assigned = (document.getElementById("t_assigned") as HTMLInputElement).value;
          let t_dd = (document.getElementById("t_dd") as HTMLInputElement).value;
          let t_comments = (document.getElementById("t_comments") as HTMLInputElement).value;

          let data =
          {
              title: t_title,
              status: t_status,
              delivery_date: t_dd,
              comments: t_comments,
              description: t_desc,
              assigned_to: t_assigned
          };

          console.log(data);


          return new Promise((resolve, reject) =>
          {
            // fetch("https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks/" + id,
            // {
            //     headers: { Authorization: "Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd" }
            // })
            fetch("http://localhost:4000/task/add/", { body: JSON.stringify(data), method: 'post', headers: { 'Content-Type': 'application/json' } })
              .then((resp) =>
              {
                // manejar el parametro status de la respuesta
                resp.json().then(result =>
                {
                    alert("Task added succesfully");
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


  },
})
</script>

<style scoped>
body, html
{

}
@keyframes fadeUp
{
    0%
    {
        opacity: 0;
        transform: translateY(20px);
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
    padding-bottom: 75px;
    box-sizing: border-box;
    position: absolute;
}
.addTaskCont
{
    width: 700px;
    height: auto;
    background: #fff;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    margin: 0 auto;
    padding: 30px;
    box-sizing: border-box;
    animation: fadeUp 1s ease-in-out;
}
.addTaskCont h3
{
    text-align: center;
    margin: 0;
    margin-bottom: 35px;
}
.addTaskCont input
{
    width: 100%;
    height: 35px;
    margin-bottom: 15px;
    border: none;
    border-bottom: 2px solid #900C3F;
}
.addTaskCont textarea
{
    width: 100%;
    height: 100px;
    margin-bottom: 15px;
    border: 2px solid #900C3F;
}
.addTaskCont button
{
    width: 200px;
    height: 35px;
    border: none;
    background: #5e27fb;
    color: #fff;
    font-size: 16px;
    margin: 0 auto;
    margin-top: 35px;
    cursor: pointer;
}

@media (max-width: 577px)
{
    .addTaskCont
    {
        width: 90%;
    }
}

</style>
