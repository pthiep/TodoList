<template>
  <div>
    <p>Completed Tasks: {{todos.filter(todo => {return todo.done === true}).length}}</p>
    <p>Pending Tasks: {{todos.filter(todo => {return todo.done === false}).length}}</p>
    <todo v-on:delete-todo="deleteTodo" v-on:complete-todo="completeTodo" v-for="todo in todos" :key="todo.id" :todo.sync="todo"></todo>
  </div>
</template>

<script type = "text/javascript">
import sweetalert from 'sweetalert';
import Todo from './Todo';

export default {
	props: ['todos'],
	components: {
		Todo,
	},
	methods: {
		deleteTodo(todo) {
			sweetalert({
				title: 'Are you sure?',
				text: 'This To-Do will be permanently deleted!',
				icon: 'warning',
				buttons: {
					cancel: true,
					confirm: {
						text: 'Yes, delete it!',
						dangerMode: true,
					},
				},
			})
				.then((willDelete) => {
					if (willDelete) {
						const todoIndex = this.todos.indexOf(todo);
						this.todos.splice(todoIndex, 1);
						sweetalert('Deleted!', 'Your To-Do has been deleted.', 'success');
					}
				});
		},
		completeTodo(todo) {
			const todoIndex = this.todos.indexOf(todo);
			this.todos[todoIndex].done = true;
			sweetalert('Success!', 'To-Do completed!', 'success');
		},
	},
};
</script>
<style scoped>
p.tasks {
  text-align: center;
}
</style>