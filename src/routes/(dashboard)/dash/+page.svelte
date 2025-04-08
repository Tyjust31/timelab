<!-- App.svelte -->
<script lang="ts">


  let tasks  = $state([
  { id: 1, title: "Réunion d'équipe", time: "09:00", completed: false },
    { id: 2, title: "Déjeuner avec client", time: "12:30", completed: false },
    { id: 3, title: "Présentation projet", time: "15:00", completed: true }
  ])


  let newTaskTitle: string = $state("");
  let newTaskTime: string = $state("");
  let nextId:number =$state(4)



  function addTask() {
    if (newTaskTitle.trim() === "" || newTaskTime.trim() === "") return;

    tasks.push({
      id: nextId,
      title: newTaskTitle,
      time: newTaskTime,
      completed: false
    })
    nextId++;
    newTaskTitle = "";
    newTaskTime = "";

  }


  
  function deleteTask(id:number) {
    tasks = tasks.filter(task => task.id !== id);
  }
  
  function toggleTaskCompletion(id: number) {
    tasks = tasks.map(task => {
      if (task.id === id) {
        return { ...task, completed: !task.completed };
      }
      return task;
    });
  }
  
  function formatTasks() {
    return [...tasks].sort((a, b) => {
      // Sort by completion status first (incomplete tasks first)
      if (a.completed !== b.completed) {
        return a.completed ? 1 : -1;
      }
      
      // Then sort by time
      return a.time.localeCompare(b.time);
    });
  }
</script>

<main class="container">
  <div class="dashboard">
    <h1>Mon Emploi du Temps</h1>
    
    <div class="add-task-form">
      <h2>Ajouter une tâche</h2>
      <div class="form-inputs">
        <input 
          type="text" 
          placeholder="Titre de la tâche" 
          bind:value={newTaskTitle}
        />
        <input 
          type="time" 
          placeholder="Heure" 
          bind:value={newTaskTime}
        />
        <button onclick={addTask}>Ajouter</button>
      </div>
    </div>
    
    <div class="tasks-container">
      <h2>Mes Tâches</h2>
      {#if tasks.length === 0}
        <p class="empty-list">Aucune tâche pour aujourd'hui</p>
      {:else}
        <ul class="task-list">
          {#each formatTasks() as task (task.id)}
            <li class={task.completed ? "task completed" : "task"}>
              <div class="task-info">
                <input 
                  type="checkbox" 
                  checked={task.completed}
                  onchange={() => toggleTaskCompletion(task.id)}
                />
                <div class="task-details">
                  <span class="task-title">{task.title}</span>
                  <span class="task-time">{task.time}</span>
                </div>
              </div>
              <button 
                class="delete-btn" 
                onclick={() => deleteTask(task.id)}
              >
                Supprimer
              </button>
            </li>
          {/each}
        </ul>
      {/if}
    </div>
  </div>
</main>

<style>
  .container {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
  }
  
  .dashboard {
    background-color: #f9f9f9;
    border-radius: 10px;
    padding: 25px;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  }
  
  h1 {
    color: #3a86ff;
    text-align: center;
    margin-bottom: 30px;
    font-size: 28px;
  }
  
  h2 {
    color: #555;
    font-size: 20px;
    margin-bottom: 15px;
    border-bottom: 2px solid #eee;
    padding-bottom: 10px;
  }
  
  .add-task-form {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 30px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  }
  
  .form-inputs {
    display: flex;
    gap: 10px;
  }
  
  input {
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 6px;
    font-size: 16px;
    flex-grow: 1;
  }
  
  button {
    background-color: #3a86ff;
    color: white;
    border: none;
    border-radius: 6px;
    padding: 12px 20px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.2s;
  }
  
  button:hover {
    background-color: #2878fc;
  }
  
  .task-list {
    list-style: none;
    padding: 0;
  }
  
  .task {
    background-color: white;
    padding: 16px;
    border-radius: 8px;
    margin-bottom: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
    transition: all 0.2s;
  }
  
  .task:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  }
  
  .task.completed {
    opacity: 0.7;
    background-color: #f0f7ff;
  }
  
  .task.completed .task-title {
    text-decoration: line-through;
    color: #888;
  }
  
  .task-info {
    display: flex;
    align-items: center;
    gap: 15px;
  }
  
  .task-details {
    display: flex;
    flex-direction: column;
  }
  
  .task-title {
    font-size: 17px;
    font-weight: 500;
  }
  
  .task-time {
    font-size: 14px;
    color: #3a86ff;
    margin-top: 5px;
  }
  
  .delete-btn {
    background-color: #ff4d6d;
    padding: 8px 15px;
    font-size: 14px;
  }
  
  .delete-btn:hover {
    background-color: #ff3961;
  }
  
  .empty-list {
    text-align: center;
    color: #888;
    font-style: italic;
    padding: 30px 0;
  }
  
  input[type="checkbox"] {
    width: 20px;
    height: 20px;
    accent-color: #3a86ff;
    cursor: pointer;
  }

  @media (max-width: 600px) {
    .form-inputs {
      flex-direction: column;
    }
    
    .task {
      flex-direction: column;
      align-items: flex-start;
      gap: 15px;
    }
    
    .delete-btn {
      align-self: flex-end;
    }
  }
</style>