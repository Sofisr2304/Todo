<script>
  import BannerDark from "$lib/images/bg-desktop-dark.jpg"
  import BannerLight from "$lib/images/bg-desktop-light.jpg"
  import IconMoon from "$lib/images/icon-moon.svg"
  import IconSun from "$lib/images/icon-sun.svg"

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
      }
      todos = [...todos, task];
      e.target.value = '';
    }
  }

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
  }

  const clearCompleted = () => {
    if (todos.length === 0) {
      return;
    } 
    todos = todos.filter(todo => !todo.completed);
    todosTemp = todos;
  }

  const changeTheme = () => {
    currentTheme = currentTheme === 'dark' ? 'light' : 'dark';
  }

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
    & .icon-theme {
      cursor: pointer;
      position: absolute;
      top: 1rem;
      right: 1rem;
    }
    & .img-banner {
      cursor: pointer;
      width: 100%;
      height: 30vh;
    }
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
    background-color: light-dark(var(--very-light-gray) , var(--very-dark-desaturated-blue));
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

  /* Todo List */
  .todo-list {
    margin-bottom: 0;
    list-style: none;
    background-color: light-dark(var(--very-light-gray), var(--very-dark-desaturated-blue));
    border-radius: 5px;
    padding: 0;
    & li {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem 1.5rem;
      border-bottom: 1px solid light-dark(var(--light-grayish-blue), var(--very-dark-grayish-blue-1));
    }
    & label {
      cursor: pointer;
      color: light-dark(var(--dark-grayish-blue), var(--light-grayish-blue));
      font-size: 18px;
    }
    & input[type="checkbox"]:checked + label {
      color: light-dark(var(--dark-grayish-blue), var(--light-grayish-blue));
      text-decoration: line-through;
    }
  }

  /* Filters */
  .todo-filters {
    background-color: light-dark(var(--very-light-gray), var(--very-dark-desaturated-blue));
    padding: 1rem 1.5rem;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
    & span {
      color: light-dark(var(--dark-grayish-blue), var(--light-grayish-blue));
    }

    & button {
      font: inherit;
      cursor: pointer;
      border: 0;
      background-color: transparent;
      color: light-dark(var(--dark-grayish-blue), var(--light-grayish-blue));
    }

    & button:hover {
      color: light-dark(var(--very-dark-grayish-blue), var(--light-grayish-blue-hover));
    }

    & button:focus {
      outline: none;
    }

    & button:active {
      transform: scale(0.95);
    }
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

<div class={`container ${currentTheme}`}>
  <div class="banner">
    <img class="img-banner" src={currentTheme === 'dark' ? BannerDark : BannerLight} alt="Dark Mode" />
    <img
      on:click={changeTheme}
      class="icon-theme"
      src={currentTheme === 'dark' ? IconSun : IconMoon} alt="Dark Mode"
    />
  </div>

  <div class="todo-container">
    <div class="todo-center">
      <input
        class="todo-input"
        type="text"
        placeholder="Create a new todo..."
        on:keypress={onKeyPress}
      >

      {#if todos.length > 0}
        <ul class="todo-list">
          {#each todos as todo}
            <li>
              <div>
                <input
                  type="checkbox"
                  id={todo.id}
                  checked={todo.completed}
                  on:change={() => todo.completed = !todo.completed}
                />
                <label for={todo.id}>{todo.text}</label>
              </div>
            </li>
          {/each}
        </ul>
    
      {/if}
    
      <div class="todo-filters">
        <span>{todosLeft} items left</span>
        <button on:click={() => filterTodos('all')} class={filterSelected === 'all' && 'active'} >All</button>
        <button on:click={() => filterTodos('active')} class={filterSelected === 'active' && 'active'}>Active</button>
        <button on:click={() => filterTodos('completed')} class={filterSelected === 'completed' && 'active'}>Completed</button>
        <button on:click={clearCompleted}>Clear Completed</button>
      </div>
    </div>
  </div>
</div>
