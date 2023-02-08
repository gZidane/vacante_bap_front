<template>
    <div class="container">
        <div class="taskContainer">
            <h2>List of tasks</h2>
            <a v-for="task of tasks" :href=" '/task/' + task.id ">
                <div class="taskItem">
                    <div class="taskItemLeft">
                        <h3>{{ task.title }}</h3>
                    </div>
                    <div class="taskItemRight">
                        <p>Delivery date: {{ task.delivery_date }}</p>
                        <p>Assigned to: {{ task.assigned_to }}</p>
                    </div>
                </div>
            </a>
        </div>

        <a href="/task/add">
            <button alt="Add new task" class="addButton" >+</button>
        </a>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'IndexPage',

  data() {
      return {
          headers: { Authorization: "Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd" },
          tasks: null
      }

  },
  methods:
  {
      getAllPost: function()
      {
          return new Promise((resolve, reject) =>
          {
            // fetch("https://ecsdevapi.nextline.mx/vdev/tasks-challenge/tasks",
            // {
            //     headers: { Authorization: "Bearer e864a0c9eda63181d7d65bc73e61e3dc6b74ef9b82f7049f1fc7d9fc8f29706025bd271d1ee1822b15d654a84e1a0997b973a46f923cc9977b3fcbb064179ecd" }
            // })
            fetch("http://localhost:4000/task/all")
            .then((resp) =>
            {
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
  },
  created()
  {

      console.log(this.headers);

      this.getAllPost()
      .then((resp) =>
      {
        this.tasks = (resp as any).data;

        setTimeout(() =>
        {
            let items = document.getElementsByClassName("taskItem");

            for(let i = 0; i < items.length; i++)
            {
                setTimeout(() =>
                {
                    (items[i] as HTMLElement).classList.add("animated");

                }, i * 250);
            }

        }, 300);

      })
      .catch(err =>
      {
        console.log(err);

      });
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
.addButton
{
    position: fixed;
    width: 60px;
    height: 60px;
    border: none;
    border-radius: 100%;
    background: #5e27fb;
    color: #fff;
    bottom: 15px;
    right: 15px;
    font-size: 32px;
    cursor: pointer;
    box-shadow: 0 5px 15px rgba(0,0,0,0.5);
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
.taskContainer
{
    width: max-content;
    margin: 0 auto;
}
.taskContainer a
{
    text-decoration: none;
}
.taskContainer h2
{
    text-align: center;
}
.taskItem
{
    width: 500px;
    height: auto;
    border-radius: 15px;
    margin-bottom: 15px;
    background: #fff;
    box-shadow: 0 3px 10px rgba(0,0,0,0.2);
    padding: 15px 10px;
    box-sizing: border-box;
    display: flex;
    color: #222;
    opacity: 0;
}
.taskItem.animated
{
    animation: fadeUp 1s ease-in-out;
    opacity: 1;
}
.taskItemLeft
{
    width: 30%;
    height: 100%;
}
.taskItemLeft h3
{
    margin: 0;
    font-size: 14px;
    color: #900C3F;
}
.taskItemRight
{
    width: 70%;
    height: 100%;
    display: flex;
    flex-direction: column;
}
.taskItemRight p
{
    font-size: 11px;
    margin: 0;
    margin-bottom: 7px;
}

@media (max-width: 577px)
{
    .container
    {
        min-height: 100%;
    }
    .taskContainer
    {
        width: 100%;
    }
    .taskItem
    {
        width: 90%;
        margin: 0 auto;
        margin-bottom: 15px;
    }
    .taskItemLeft h3
    {
        font-size: 11px;
    }
}

</style>
