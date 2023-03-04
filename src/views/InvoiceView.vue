<template>
	<div
		v-if="currentInvoice"
		class="invoice-view container"
	>
		<router-link
			class="nav-link flex"
			:to="{ name: 'home' }"
		>
			<img
				src="@/assets/icon-arrow-left.svg"
				alt=""
			/>
			Go Back
		</router-link>
		<!-- Header -->
		<div class="header flex">
			<div class="left flex">
				<span>Status</span>
				<div
					class="status-button flex"
					:class="{
						paid: currentInvoice.invoicePaid,
						draft: currentInvoice.invoiceDraft,
						pending: currentInvoice.invoicePending,
					}"
				>
					<span v-if="currentInvoice.invoicePaid">Paid</span>
					<span v-if="currentInvoice.invoiceDraft">Draft</span>
					<span v-if="currentInvoice.invoicePending">Pending</span>
				</div>
			</div>
			<div class="right flex">
				<button
					@click="toggleEditInvoice"
					class="dark-purple"
				>
					Edit
				</button>
				<button
					@click="deleteInvoice(currentInvoice.docId)"
					class="red"
				>
					Delete
				</button>
				<button
					@click="updateStatusToPaid(currentInvoice.docId)"
					v-if="currentInvoice.invoicePending"
					class="green"
				>
					Mark as Paid
				</button>
				<button
					v-if="currentInvoice.invoiceDraft || currentInvoice.invoicePaid"
					@click="updateStatusToPending(currentInvoice.docId)"
					class="orange"
				>
					Mark as Pending
				</button>
			</div>
		</div>

		<!-- Invoice Details -->
		<div class="invoice-details flex flex-column">
			<div class="top flex">
				<div class="left flex flex-column">
					<p><span>#</span>{{ currentInvoice.invoiceId }}</p>
					<p>{{ currentInvoice.productDescription }}</p>
				</div>
				<div class="right flex flex-column">
					<p>{{ currentInvoice.billerStreetAddress }}</p>
					<p>{{ currentInvoice.billerCity }}</p>
					<p>{{ currentInvoice.billerZipCode }}</p>
					<p>{{ currentInvoice.billerCountry }}</p>
				</div>
			</div>
			<div class="middle flex">
				<div class="payment flex flex-column">
					<h4>Invoice Date</h4>
					<p>
						{{ currentInvoice.invoiceDate }}
					</p>
					<h4>Payment Date</h4>
					<p>
						{{ currentInvoice.paymentDueDate }}
					</p>
				</div>
				<div class="bill flex flex-column">
					<h4>Bill To</h4>
					<p>{{ currentInvoice.clientName }}</p>
					<p>{{ currentInvoice.clientStreetAddress }}</p>
					<p>{{ currentInvoice.clientCity }}</p>
					<p>{{ currentInvoice.clientZipCode }}</p>
					<p>{{ currentInvoice.clientCountry }}</p>
				</div>
				<div class="send-to flex flex-column">
					<h4>Sent To</h4>
					<p>{{ currentInvoice.clientEmail }}</p>
				</div>
			</div>
			<div class="bottom flex flex-column">
				<div class="billing-items">
					<div class="heading flex">
						<p>Item Name</p>
						<p>QTY</p>
						<p>Price</p>
						<p>Total</p>
					</div>
					<div
						v-for="(item, index) in currentInvoice.invoiceItemList"
						:key="index"
						class="item flex"
					>
						<p>{{ item.itemName }}</p>
						<p>{{ item.qty }}</p>
						<p>{{ item.price }}</p>
						<p>{{ item.total }}</p>
					</div>
				</div>
				<div class="total flex">
					<p>Amount Due</p>
					<p>{{ currentInvoice.invoiceTotal }}</p>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import { mapMutations, mapState } from 'vuex';
export default {
	name: 'InvoiceView',
	data() {
		return {
			currentInvoice: null,
		};
	},
	created() {
		this.getCurrentInvoice();
	},
	components: {},
	methods: {
		...mapMutations(['SET_CURRENT_INVOICE']),

		getCurrentInvoice() {
			this.SET_CURRENT_INVOICE(this.$route.params.invoiceId);
			this.currentInvoice = this.currentInvoiceArray[0];
		},
	},
	computed: {
		...mapState(['currentInvoiceArray']),
	},
};
</script>

<style>

</style>
