<template>
  <main class="medium-12 login">
    <section class="modal_login login-box row align-center">
      <div class="small-12 header_model">
        <img
          class="header_logo"
          src="../../assets/images/synLIFE_login_white.svg"
          alt="SynLife"
        />
      </div>
      <div v-if="!email" class=" form_style">
        <div class="div-authentication">
          <GoogleOAuthButton
            v-if="showGoogleOAuth()"
            button-size="large"
            class="oauth-reverse"
          />
          <form class="form-login" @submit.prevent="login()">
            <label
              :class="{ error: $v.credentials.email.$error }"
              class="input-email"
            >
              <p class="">
                {{ $t('LOGIN.EMAIL.LABEL') }}
              </p>
              <input
                v-model.trim="credentials.email"
                type="text"
                data-testid="email_input"
                :placeholder="$t('LOGIN.EMAIL.PLACEHOLDER')"
                @input="$v.credentials.email.$touch"
              />
            </label>
            <label
              :class="{ error: $v.credentials.password.$error }"
              class="input-password"
            >
              <p>
                {{ $t('LOGIN.PASSWORD.LABEL') }}
              </p>

              <input
                v-model.trim="credentials.password"
                type="password"
                data-testid="password_input"
                :placeholder="$t('LOGIN.PASSWORD.PLACEHOLDER')"
                @input="$v.credentials.password.$touch"
              />
            </label>
            <woot-submit-button
              :disabled="
                $v.credentials.email.$invalid ||
                  $v.credentials.password.$invalid ||
                  loginApi.showLoading
              "
              :button-text="$t('LOGIN.SUBMIT')"
              :loading="loginApi.showLoading"
              button-class="large expanded"
            />
          </form>
          <p
            v-if="!globalConfig.disableUserProfileUpdate"
            class="text-center forgot_password align-bottom"
          >
            {{ $t('LOGIN.FORGOT_PASSWORD') }}
            <router-link to="auth/reset/password">
              {{ $t('LOGIN.CLICK_HERE') }}
            </router-link>
          </p>
        </div>
        <!--        <div class="text-center column sigin__footer">-->
        <!--          <p v-if="!globalConfig.disableUserProfileUpdate">-->
        <!--            <router-link to="auth/reset/password">-->
        <!--              {{ $t('LOGIN.FORGOT_PASSWORD') }}-->
        <!--            </router-link>-->
        <!--          </p>-->
        <!--          <p v-if="showSignupLink()">-->
        <!--            <router-link to="auth/signup">-->
        <!--              {{ $t('LOGIN.CREATE_NEW_ACCOUNT') }}-->
        <!--            </router-link>-->
        <!--          </p>-->
        <!--        </div>-->
      </div>
      <woot-spinner v-else size="" />
    </section>
    <footer>
      Feito por SynLife
    </footer>
  </main>
</template>

<script>
import { required, email } from 'vuelidate/lib/validators';
import globalConfigMixin from 'shared/mixins/globalConfigMixin';
import WootSubmitButton from 'components/buttons/FormSubmitButton';
import { mapGetters } from 'vuex';
import { parseBoolean } from '@chatwoot/utils';
import GoogleOAuthButton from '../../components/ui/Auth/GoogleOAuthButton.vue';

const ERROR_MESSAGES = {
  'no-account-found': 'LOGIN.OAUTH.NO_ACCOUNT_FOUND',
  'business-account-only': 'LOGIN.OAUTH.BUSINESS_ACCOUNTS_ONLY',
};

export default {
  components: {
    WootSubmitButton,
    GoogleOAuthButton,
  },
  mixins: [globalConfigMixin],
  props: {
    ssoAuthToken: { type: String, default: '' },
    ssoAccountId: { type: String, default: '' },
    ssoConversationId: { type: String, default: '' },
    config: { type: String, default: '' },
    email: { type: String, default: '' },
    authError: { type: String, default: '' },
  },
  data() {
    return {
      // We need to initialize the component with any
      // properties that will be used in it
      credentials: {
        email: '',
        password: '',
      },
      loginApi: {
        message: '',
        showLoading: false,
      },
      error: '',
    };
  },
  validations: {
    credentials: {
      password: {
        required,
      },
      email: {
        required,
        email,
      },
    },
  },
  computed: {
    ...mapGetters({
      globalConfig: 'globalConfig/get',
    }),
  },
  created() {
    if (this.ssoAuthToken) {
      this.login();
    }
    if (this.authError) {
      const message = ERROR_MESSAGES[this.authError] ?? 'LOGIN.API.UNAUTH';
      this.showAlert(this.$t(message));
      // wait for idle state
      window.requestIdleCallback(() => {
        // Remove the error query param from the url
        const { query } = this.$route;
        this.$router.replace({ query: { ...query, error: undefined } });
      });
    }
  },
  methods: {
    showAlert(message) {
      // Reset loading, current selected agent
      this.loginApi.showLoading = false;
      this.loginApi.message = message;
      bus.$emit('newToastMessage', this.loginApi.message);
    },
    showSignupLink() {
      return parseBoolean(window.chatwootConfig.signupEnabled);
    },
    showGoogleOAuth() {
      return Boolean(window.chatwootConfig.googleOAuthClientId);
    },
    login() {
      this.loginApi.showLoading = true;
      const credentials = {
        email: this.email
          ? decodeURIComponent(this.email)
          : this.credentials.email,
        password: this.credentials.password,
        sso_auth_token: this.ssoAuthToken,
        ssoAccountId: this.ssoAccountId,
        ssoConversationId: this.ssoConversationId,
      };
      this.$store
        .dispatch('login', credentials)
        .then(() => {
          this.showAlert(this.$t('LOGIN.API.SUCCESS_MESSAGE'));
        })
        .catch(response => {
          // Reset URL Params if the authentication is invalid
          if (this.email) {
            window.location = '/app/login';
          }

          if (response && response.status === 401) {
            const { errors } = response.data;
            const hasAuthErrorMsg =
              errors &&
              errors.length &&
              errors[0] &&
              typeof errors[0] === 'string';
            if (hasAuthErrorMsg) {
              this.showAlert(errors[0]);
            } else {
              this.showAlert(this.$t('LOGIN.API.UNAUTH'));
            }
            return;
          }
          this.showAlert(this.$t('LOGIN.API.ERROR_MESSAGE'));
        });
    },
  },
};
</script>

<style lang="scss" scoped>
@import '~dashboard/assets/scss/variables';
.oauth-reverse {
  display: flex;
  flex-direction: column-reverse;
}
main {
  background-image: linear-gradient(
      360deg,
      rgba(42, 20, 129, 0.8) 15.63%,
      rgba(73, 26, 166, 0.7328) 41.15%,
      rgba(116, 35, 217, 0.64) 67.71%,
      rgba(165, 17, 220, 0.8) 100%
    ),
    url('../../assets/images/milad-fakurian-PGdW_bHDbpI-unsplash.svg');
  background-repeat: no-repeat;
  background-size: cover;
}

footer {
  //float: left;
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  font-size: 1.6rem;
  font-weight: $font-weight-bold;
  color: white;
  text-align: center;
  margin-bottom: 4rem;
}
</style>
