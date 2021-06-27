<template>
  <section>
    <div class="buttons">
      <b-button
        label="Launch card modal (Component)"
        type="is-primary"
        size="is-medium"
        @click="cardModal"
      />
    </div>
  </section>
</template>

<script>
const ModalForm = {
  props: ["name"],
  template: `
            <form action="">
                <div class="modal-card" style="width: auto">
                    <header class="modal-card-head">
                        <p class="modal-card-title">Login</p>
                        <button
                            type="button"
                            class="delete"
                            @click="$emit('close')"/>
                    </header>
                    <section class="modal-card-body">
                        <b-field label="name">
                            <b-input
                                type="name"
                                :value="name"
                                placeholder="Your name"
                                required>
                            </b-input>
                        </b-field>

                        <b-field label="Password">
                            <b-input
                                type="password"
                                :value="password"
                                password-reveal
                                placeholder="Your password"
                                required>
                            </b-input>
                        </b-field>

                        <b-checkbox>Remember me</b-checkbox>
                    </section>
                    <footer class="modal-card-foot">
                        <b-button
                            label="Close"
                            @click="$emit('close')" />
                        <b-button
                            label="Login"
                            type="is-primary" />
                    </footer>
                </div>
            </form>
        `,
};

export default {
  methods: {
    cardModal() {
      this.$buefy.modal.open({
        parent: this,
        component: ModalForm,
        hasModalCard: true,
        customClass: "custom-class custom-class-2",
        trapFocus: true,
      });
    },
  },
};
</script>
