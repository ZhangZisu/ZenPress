<template>
	<v-container fluid>
		<v-layout>
			<v-flex>
				<v-form v-model="valid">
					<v-card>
						<v-card-title primary-title>
							<div>
								<div class="headline" v-text="$t('modify_site_password')"/>
								<div class="subheading" v-text="$t('modify_site_password_subtitle')"/>
							</div>
						</v-card-title>
						<v-card-text>
							<v-text-field v-model="password" :rules="[notEmpty]" :label="$t('site_password')" type="password"/>
						</v-card-text>
						<v-card-actions>
							<v-spacer/>
							<v-btn depressed color="secondary" @click.stop="$router.go(-1)" v-text="$t('cancel')"/>
							<v-btn depressed color="primary" @click.stop="submit" v-text="$t('submit')"/>
						</v-card-actions>
					</v-card>
				</v-form>
			</v-flex>
		</v-layout>
	</v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "site_password_edit",
  data() {
    return {
      password: "",
      valid: false
    };
  },
  methods: {
    notEmpty(str) {
      return (str && str.length >= 0) || "Cannot be empty";
    },
    async submit() {
      try {
        this.$store.commit("querying", true);
        await axios.put("/site/modify/password", { password: this.password });
        this.$store.commit("querying", false);
        this.$store.dispatch("logout");
        this.$store.dispatch("loadSite");
        this.$router.push("/");
      } catch (e) {
        this.$store.commit("querying", false);
        this.$store.commit("error_status", true);
        this.$store.commit("error_text", e.message);
      }
    }
  }
};
</script>
