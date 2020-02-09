<template>
	<div id="employee-table">
		<p v-if="employees.length < 1" class="empty-table">
			No Employees
		</p>
		<table v-else>
			<thead>
				<tr>
					<th>Username</th>
					<th>Email</th>
					<th>Action</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="employee in employees" :key="employee.id">
					<td v-if="editing === employee.id">
						<input type="text" v-model="employee.username" />
					</td>
					<td v-else>{{ employee.username }}</td>
					<td v-if="editing === employee.id">
						<input type="text" v-model="employee.email" />
					</td>
					<td v-else>{{ employee.email }}</td>
					<td v-if="editing === employee.id">
						<button @click="editEmployee(employee)">Save</button>
						<button class="muted-button" @click="editing = null">Cancel</button>
					</td>
					<td v-else>
						<button @click="editMode(employee)">
							Edit
						</button>
						&nbsp;
						<button @click="$emit('delete:employee', employee.id)">
							Delete
						</button>
					</td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

<script>
	export default {
		name: 'employee-table',
		props: {
			employees: Array,
		},
		data() {
			return {
				editing: null,
			}
		},
		methods: {
			editMode(employee) {
				this.cachedEmployee = Object.assign({}, employee)
				this.editing = employee.id
			},

			cancelEdit(employee) {
				Object.assign(employee, this.cachedEmployee)
				this.editing = null;
			},

			editEmployee(employee) {
				if (employee.username === '' || employee.email === '') return
				this.$emit('edit:employee', employee.id, employee)
				this.editing = null
			}
		}
	}

</script>

<style scoped></style>
