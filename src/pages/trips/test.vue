<template>
  <div>
    <h1>Create Customer</h1>
    <form @submit.prevent="handleSubmit">
      <label>
        First Name:
        <input
          v-model="firstName"
          type="text"
          required
        >
      </label>
      <br>
      <label>
        Last Name:
        <input
          v-model="lastName"
          type="text"
          required
        >
      </label>
      <br>
      <label>
        Birthdate:
        <input
          v-model="birthdate"
          type="date"
          required
        >
      </label>
      <br>
      <label>
        Trip ID:
        <input
          v-model="tripId"
          type="text"
          required
        >
      </label>
      <br>
      <button type="submit">
        Create Customer
      </button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      firstName: '',
      lastName: '',
      birthdate: '',
      tripId: '',
    }
  },
  methods: {
    async handleSubmit() {
      const customerData = {
        fname: this.firstName,
        lname: this.lastName,
        birthdate: this.birthdate,
      }

      const response = await this.createCustomer(this.tripId, customerData)
      if (response) {
        console.log('Customer created:', response)
      } else {
        console.error('Failed to create customer')
      }
    },
    async createCustomer(tripId, customerData) {
      try {
        return await this.$axios.$post('/customers', {
          ...customerData,
          trips: [tripId],
        })
      } catch (error) {
        console.error(error)
        
        return null
      }
    },
  },
}
</script>
