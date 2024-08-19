<script>
  import BannerDark from "$lib/images/bg-desktop-dark.jpg";
  import BannerLight from "$lib/images/bg-desktop-light.jpg";
  import IconCross from "$lib/images/icon-cross.svg";
  import IconPencil from "$lib/images/icon-pencil.svg";
  import IconMoon from "$lib/images/icon-moon.svg";
  import IconSun from "$lib/images/icon-sun.svg";

  let todos = [];
  let todosTemp = [];
  let filterSelected = 'all';
  let currentTheme = 'dark';

  const onKeyPress = (e) => {
    if (e.key === 'Enter') {
      const task = {
        id: Math.random(),
        text: e.target.value,
        completed: false
      };
      todos = [...todos, task];
      e.target.value = '';
    }
  };

  const filterTodos = (filter) => {
    filterSelected = filter;

    if (todosTemp.length === 0) {
      todosTemp = [...todos];
    } else {
      todos = [...todosTemp];
    }
    if (filter === 'active') {
      todos = todosTemp.filter(todo => !todo.completed);
    } else if (filter === 'completed') {
      todos = todosTemp.filter(todo => todo.completed);
    } else {
      todos = todosTemp;
    }
  };

  const clearCompleted = () => {
    if (todos.length === 0) {
      return;
    }
    todos = todos.filter(todo => !todo.completed);
    todosTemp = todos;
  };

  const changeTheme = () => {
    currentTheme = currentTheme === 'dark' ? 'light' : 'dark';
  };

  const deleteTask = (id) => {
    todos = todos.filter(todo => todo.id !== id);
    todosTemp = todos;
  };

  $: todosLeft = todos.filter(todo => !todo.completed).length;
</script>

<style>
  :root {
    color-scheme: light dark;
    --bright-blue: hsl(220, 98%, 61%);
    --check-background: linear-gradient(90deg, hsl(192, 100%, 67%), hsl(280, 87%, 65%));
    
    /* Light Theme */
    --very-light-gray: hsl(0, 0%, 98%);
    --very-light-grayish-blue: hsl(236, 33%, 92%);
    --light-grayish-blue: hsl(233, 11%, 84%);
    --dark-grayish-blue: hsl(236, 9%, 61%);
    --very-dark-grayish-blue: hsl(235, 19%, 35%);

    /* Dark Theme */
    --very-dark-blue: hsl(235, 21%, 11%);
    --very-dark-desaturated-blue: hsl(235, 24%, 19%);
    --light-grayish-blue: hsl(234, 39%, 85%);
    --light-grayish-blue-hover: hsl(236, 33%, 92%);
    --dark-grayish-blue: hsl(234, 11%, 52%);
    --very-dark-grayish-blue-1: hsl(233, 14%, 35%);
    --very-dark-grayish-blue-2: hsl(237, 14%, 26%);
  }

  :global(body) {
    margin: 0;
    padding: 0;
    font-family: "Josefin Sans", sans-serif;
    font-optical-sizing: auto;
    font-weight: 400;
    font-style: normal;
  }

  .banner {
    height: 30vh;
    position: relative;
  }

  .icon-theme {
    cursor: pointer;
    position: absolute;
    top: 1rem;
    right: 1rem;
  }

  .img-banner {
    cursor: pointer;
    width: 100%;
    height: 30vh;
  }

  .todo-container {
    height: 70vh;
    background-color: light-dark(var(--very-light-grayish-blue), var(--very-dark-blue));
    display: flex;
    justify-content: center;
  }

  .todo-input {
    width: 90%;
    margin-bottom: 2rem;
    font: inherit;
    outline: none;
    color: light-dark(var(--very-dark-grayish-blue), var(--light-grayish-blue));
    background-color: light-dark(var(--very-light-gray), var(--very-dark-desaturated-blue));
    font-size: 2rem;
    padding: 1rem 1.5rem;
    border: 0;
    box-shadow: 1px 5px 10px rgba(0, 0, 0, 0.1);
    border-radius: 5px;
  }

  .todo-center {
    width: 100%;
    max-width: 500px;
    padding: 2rem;
  }

  .todo-list {
    margin-bottom: 0;
    list-style: none;
    background-color: light-dark(var(--very-light-gray), var(--very-dark-desaturated-blue));
    border-radius: 5px;
    padding: 0;
  }

  .todo-list li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 1.5rem;
    border-bottom: 1px solid light-dark(var(--light-grayish-blue), var(--very-dark-grayish-blue-1));
  }

  .label-task {
    display: flex;
    align-items: center;
  }

  .label-task label {
    cursor: pointer;
    color: light-dark(var(--dark-grayish-blue), var(--light-grayish-blue));
    font-size: 18px;
  }

  .label-task input[type="checkbox"]:checked + label {
    color: light-dark(var(--dark-grayish-blue), var(--light-grayish-blue));
    text-decoration: line-through;
  }

  .input-edit-task {
    font: inherit;
    outline: none;
    color: light-dark(var(--very-dark-grayish-blue), var(--light-grayish-blue));
    background-color: light-dark(var(--very-light-gray), var(--very-dark-desaturated-blue));
    font-size: 18px;
    padding: 0.5rem 1rem;
    border: 0;
    box-shadow: 1px 5px 10px rgba(0, 0, 0, 0.1);
    border-radius: 5px;
  }

  .buttons-task {
    display: flex;
    align-items: center;
  }

  .buttons-task button {
    cursor: pointer;
    background-color: transparent;
    border: 0;
  }

  .todo-filters {
    background-color: light-dark(var(--very-light-gray), var(--very-dark-desaturated-blue));
    padding: 1rem 1.5rem;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .todo-filters span {
    color: light-dark(var(--dark-grayish-blue), var(--light-grayish-blue));
  }

  .todo-filters button {
    font: inherit;
    cursor: pointer;
    border: 0;
    background-color: transparent;
    color: light-dark(var(--dark-grayish-blue), var(--light-grayish-blue));
  }

  .todo-filters button:hover {
    color: light-dark(var(--very-dark-grayish-blue), var(--light-grayish-blue-hover));
  }

  .todo-filters button:focus {
    outline: none;
  }

  .todo-filters button:active {
    transform: scale(0.95);
  }

  .icon-cross {
    cursor: pointer;
    position: absolute;
  }

  .active {
    color: var(--bright-blue)!important;
  }

  .dark {
    color-scheme: dark;
  }

  .light {
    color-scheme: light;
  }
</style>

<section class={`container ${currentTheme}`}>
  <header class="banner">
    <img class="img-banner" src={currentTheme === 'dark' ? BannerDark : BannerLight} alt="Dark Mode" />
    <img
      on:click={changeTheme}
      class="icon-theme"
      src={currentTheme === 'dark' ? IconSun : IconMoon} alt="Dark Mode"
    />
  </header>

  <main class="todo-container">
    <div class="todo-center">
      <input
        class="todo-input"
        type="text"
        placeholder="Create a new todo..."
        on:keypress={onKeyPress}
      />

      {#if todos.length > 0}
        <ul class="todo-list">
          {#each todos as todo}
            <li>
              <div class="label-task">
                <input
                  type="checkbox"
                  id={todo.id}
                  checked={todo.completed}
                  on:change={() => todo.completed = !todo.completed}
                />
                {#if todo.isEditing}
                  <input
                    class="input-edit-task"
                    type="text"
                    value={todo.text}
                    on:input={(e) => todo.text = e.target.value}
                    on:blur={() => todo.isEditing = false}
                    on:keypress={(e) => {
                      if (e.key === 'Enter') {
                        todo.isEditing = false;
                      }
                    }}
                    id={`${todo.id}-edit`}
                  />
                {:else}
                  <label for={todo.id}>{todo.text}</label>
                {/if}
              </div>
              <div class="buttons-task">
                <button class="button-edit-task" on:click={() => {
                  todo.isEditing = !todo.isEditing;
                  setTimeout(() => {
                    const input = document.getElementById(`${todo.id}-edit`);
                    if (todo.isEditing && input) {
                      input.focus();
                    }
                  }, 0);
                }}>
                  <img src={IconPencil} alt="edit task">
                </button>
                <button class="delete-task" on:click={deleteTask(todo.id)}>
                  <img src={IconCross} alt="delete task">
                </button>
              </div>
            </li>
          {/each}
        </ul>
      {/if}

      <footer class="todo-filters">
        <span>{todosLeft} items left</span>
        <button on:click={() => filterTodos('all')} class={filterSelected === 'all' && 'active'}>All</button>
        <button on:click={() => filterTodos('active')} class={filterSelected === 'active' && 'active'}>Active</button>
        <button on:click={() => filterTodos('completed')} class={filterSelected === 'completed' && 'active'}>Completed</button>
        <button on:click={clearCompleted}>Clear Completed</button>
      </footer>
    </div>
  </main>
</section>
