<template>
  <div class = "app">
    <nav class="navbar navbar-expand-lg">
      <div class="container-fluid">
        <a class="navbar-brand" href="/">D-Shop</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <!-- <li class="nav-item">
              <router-link class="nav-link" to="/">home</router-link>
            </li> -->
            <li class="nav-item">
              <router-link class="nav-link active" to="/"
                > Product List</router-link
              >
            </li>
            <!-- <li class="nav-item">
              <router-link class="nav-link active" to="/detail"
                >제품상세페이지</router-link
              >
            </li> -->
            <li v-if="user.email!=undefined" class="nav-item">
              <router-link class="nav-link active" to="/sales"
                > Product registration </router-link
              >
            </li>
          </ul>
          <form class="d-flex">
            <input
              class="form-control me-2 btn-outline-secondary"
              type="search"
              placeholder="Search"
              aria-label="Search"
            />
            <button class="btn btn-outline-secondary" type="submit">
              Search
            </button>
          </form>

          <ul class ="LoginBtn">
            <li v-if="user.email == undefined">
              <button class="btn btn btn-warning" type="button" @click="kakaoLogin">
                Log In
              </button>
            </li>
            <li v-else>
              <button class="btn btn btn-warning" type="button" @click="kakaoLogout">
                Log Out
              </button>
            </li>
          </ul>
          
        </div>
      </div>
    </nav>

    <router-view />

    <!-- footer -->
    <footer class="mt-5 py-5 bg-dark">
      <div class="row">
        <div class="col-12 col-md">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            fill="none"
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            class="d-block mb-2"
            role="img"
            viewBox="0 0 24 24"
          >
            <title>Product</title>
            <circle cx="12" cy="12" r="10" />
            <path
              d="M14.31 8l5.74 9.94M9.69 8h11.48M7.38 12l5.74-9.94M9.69 16L3.95 6.06M14.31 16H2.83m13.79-4l-5.74 9.94"
            />
          </svg>
          <small class="d-block mb-3 text-muted">&copy; 2017-2021</small>
        </div>
        <div class="col-6 col-md">
          <h5>Features</h5>
          <ul class="list-unstyled text-small">
            <li><a class="link-secondary" href="#">Cool stuff</a></li>
            <li><a class="link-secondary" href="#">Random feature</a></li>
            <li><a class="link-secondary" href="#">Team feature</a></li>
            <li><a class="link-secondary" href="#">Stuff for developers</a></li>
            <li><a class="link-secondary" href="#">Another one</a></li>
            <li><a class="link-secondary" href="#">Last time</a></li>
          </ul>
        </div>
        <div class="col-6 col-md">
          <h5>Resources</h5>
          <ul class="list-unstyled text-small">
            <li><a class="link-secondary" href="#">Resource name</a></li>
            <li><a class="link-secondary" href="#">Resource</a></li>
            <li><a class="link-secondary" href="#">Another resource</a></li>
            <li><a class="link-secondary" href="#">Final resource</a></li>
          </ul>
        </div>
        <div class="col-6 col-md">
          <h5>Resources</h5>
          <ul class="list-unstyled text-small">
            <li><a class="link-secondary" href="#">Business</a></li>
            <li><a class="link-secondary" href="#">Education</a></li>
            <li><a class="link-secondary" href="#">Government</a></li>
            <li><a class="link-secondary" href="#">Gaming</a></li>
          </ul>
        </div>
        <div class="col-6 col-md">
          <h5>About</h5>
          <ul class="list-unstyled text-small">
            <li><a class="link-secondary" href="#">Team</a></li>
            <li><a class="link-secondary" href="#">Locations</a></li>
            <li><a class="link-secondary" href="#">Privacy</a></li>
            <li><a class="link-secondary" href="#">Terms</a></li>
          </ul>
        </div>
      </div>
    </footer>
  </div>
</template>



<script>
export default {
  computed: {
    user() {
      return this.$store.state.user;
    },
  },
  methods: {
    kakaoLogin() {
      window.Kakao.Auth.login({
        scope: "profile, account_email, gender",
        success: this.getProfile,
      });
    },
    getProfile(authObj) {
      console.log(authObj);
      window.Kakao.API.request({
        url: "/v2/user/me",
        success: (response) => {
          const kakao_account = response.kakao_account;
          console.log(kakao_account);
          this.login(kakao_account);
          alert(" login success! ");
        },
      });
    },
  async login(kakao_account) {
      await this.$api("/api/login", {
        param: [
          {email:kakao_account.email, nickname:kakao_account.profile.nickname},
          {nickname:kakao_account.profile.nickname}
        ]
      });
      this.$store.commit("user", kakao_account);
    },
    kakaoLogout() {
      window.Kakao.Auth.logout((response) => {
        console.log(response);  //define오류 때문에 
        this.$store.commit("user", {});
        alert("로그아웃");
        this.$router.push({path:'/'}); 
      });
    }
  }
}

</script>


<style>
ol, ul {
    list-style: none;
    margin:0px; padding:0px;
}

.LoginBtn{
  margin-left: 5px;
  margin-top: 15px;
}

.navbar {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.navbar {
  background-color: #FFFFE0;
}

.navbar-brand{
  color: black;
  font-weight: bold;
}

.navbar a.router-link-exact-active{
  color:#FF4500;
}

.nav-item .nav-link{
  color: black;
}



/* #nav {
  padding: 30px;

}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: ;
} */
</style>
