<template>
	<div>
		<v-data-table :headers="headers" :items="customers" class="elevation-1">
			<template v-slot:top>
				<v-toolbar flat>
					<v-toolbar-title>Listado clientes</v-toolbar-title>
					<v-divider class="mx-4" inset vertical></v-divider>
					<v-spacer />
					<v-dialog v-model="dialog" max-width="500px">
						<template v-slot:activator="{ on, attrs }">
							<v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
								Nuevo cliente
							</v-btn>
						</template>
						<v-card>
							<v-card-title>
								<span class="text-h5">{{ formTitle }}</span>
							</v-card-title>
							<v-card-text>
								<v-container>
									<v-row>
										<v-col cols="12" sm="6" md="4">
											<v-text-field
												v-model="newItem.name"
												label="Name"
											></v-text-field>
										</v-col>
									</v-row>
								</v-container>
							</v-card-text>

							<v-card-actions>
								<v-spacer></v-spacer>
								<v-btn color="blue darken-1" text @click="close">
									Cancelar
								</v-btn>
								<v-btn color="blue darken-1" text @click="save">Guardar</v-btn>
							</v-card-actions>
						</v-card>
					</v-dialog>
				</v-toolbar>
			</template>
			<!-- <template v-slot:item.actions="{ item }">
				<v-icon small class="mr-2" @click="verClientes(item)">
					mdi-eye-open
				</v-icon>
			</template> -->
			<template v-slot:no-data>
				<v-btn color="primary" @click="load">Reset</v-btn>
			</template>
		</v-data-table>
	</div>
</template>

<script>
	import axios from 'axios';
	export default {
		name: 'CustomerList',
		mounted() {
			this.load();
		},
		methods: {
			load() {
				console.log('cargando..');
				axios
					.get('https://testingweb.free.beeceptor.com/customers')
					.then((response) => {
						if (response.data) {
							this.customers = response.data;
						} else {
							console.log('Error cargando datos...');
						}
					});
			},

			verClientes(item) {
				this.editedIndex = this.customer.indexOf(item);
				this.editedItem = Object.assign({}, item);
				this.dialog = true;
			},

			close() {
				this.dialog = false;
				this.$nextTick(() => {
					this.editedItem = Object.assign({}, this.defaultItem);
					this.editedIndex = -1;
				});
			},

			save() {
				const newCustomer = {
					name: this.newItem.name,
				};

				axios
					.post('https://testingweb.free.beeceptor.com/customers', newCustomer)
					.then((response) => {
						if (response.data) {
							this.customers.push(response.data);
						}
					})
					.catch((error) => {
						console.error(error);
					});
				this.close();
			},
		},
		data: () => ({
			dialog: false,
			dialogDelete: false,
			headers: [
				{
					text: 'ID',
					align: 'start',
					value: 'id',
				},
				{ text: 'Name', value: 'name' },
			],
			customers: [],
			editedIndex: -1,
			newItem: {
				name: '',
			},
		}),

		computed: {
			formTitle() {
				return this.editedIndex === -1 ? 'New Item' : 'Edit Item';
			},
		},

		watch: {
			dialog(val) {
				val || this.close();
			},
		},
	};
</script>
