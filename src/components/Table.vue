<template>
	<table class="table table-striped">
		<thead v-if="head && first_keys.length">
			<tr>
				<slot name="head" :keys="first_keys">
					<th scope="col">#</th>
					<th v-for="key in first_keys" scope="col">
						{{ key }}
					</th>
				</slot>
			</tr>
		</thead>

		
		<tbody>
			<tr v-for="(item, index) in items" :key="index">
				<slot name="item" :item="item" :index="index">
					<td>{{ index }}</td>
					<td v-for="(value, key) in item">
						{{ value }}
					</td>
				</slot>
			</tr>
		</tbody>

		<slot name="foot">
			<tfoot></tfoot>
		</slot>
	</table>
</template>

<script>
export default {
	props: {
		items: Array,
		head: {
			default: true,
			type: Boolean
		}	
	},

	computed: {
		first_keys() {
			return (this.items.length > 0) ? Object.keys(this.items[0]) : [];
		}
	}
}
</script>