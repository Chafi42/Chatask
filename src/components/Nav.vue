<template>

    
    <header>
    <nav class="fixed w-full z-20 top-0 start-0 ">
        <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
        <a href="../../index.html" class="flex items-center space-x-2 rtl:space-x-reverse">
           <img src="../icon/tache-bleu-removebg-preview.png" class="h-12" alt="Task">
            <span class="self-center text-2xl font-semibold whitespace-nowrap dark:text-white">Task</span>
        </a>
        </div>
      </nav>
    </header>
    
  </template>

  <script>
  export default {
    name: 'NavComponent'
  }

  const spanTitle = ref('Task')

  const changeTitle = () => {
    spanTitle.value = 'Task'
  }

  onMounted(() => {
    changeTitle()
  })

  
  </script>
