<template>
    <layout>
        <sui-message class="ui error message"
                     v-if="error !== null && errorVisible"
                     :content="error"
                     dismissable
                     @dismiss="handleDismiss"
        />
        <div class="ui text container">
            <div class="ui one column grid">
                <div class="column">
                    <h1 class="ui header">Register</h1>
                </div>
            </div>
            <div class="ui text container">
                <form @submit="register" class="ui form" method="post" action="">
                    <div class="field">
                        <label>Email</label>
                        <input type="text" required name="email" v-model="email" placeholder="user@cbsctf.live">
                    </div>
                    <div class="field">
                        <label>Password</label>
                        <input type="password" id="password" required name="password" v-model="password" placeholder="">
                    </div>
                    <button class="ui button" type="submit">Submit</button>
                </form>
            </div>
        </div>
    </layout>
</template>
<script>
    import Layout from './Layout';

    export default {
        components: {
            Layout
        },
        methods: {
            async register(event) {
                event.preventDefault();
                try {
                    await this.$http.post('register', {
                        email: this.email,
                        password: this.password,
                    });
                    this.$store.commit('login', {user: this.email});
                    this.$router.push({name: 'Home'}).catch(() => {
                    });
                } catch (error) {
                    this.error = error.response.data.error;
                    this.errorVisible = true;
                }
            },
            handleDismiss() {
                this.errorVisible = false;
            }
        },
        data: function () {
            return {
                email: null,
                password: null,
                error: null,
                errorVisible: false,
            };
        },
    };
</script>
