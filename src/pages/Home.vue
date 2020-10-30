  <template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <!-- errors -->
        <div class="error" v-if="error" style="margin-bottom: 20px">
          <p>{{ error }}</p>
        </div>

        <!-- search -->
        <search :value="search" placeholder="Type user name" @search="search = $event" />
        <br />
        <!-- buttons -->
        <button v-if="!user" class="btn btnPrimary" @click="getUser">Search</button>
        <button v-else class="btn btnPrimary" @click="getUser">Search again</button>
        <!-- userWrapper -->
        <div class="user__wrapper" v-if="user">
          <!-- userAvatar -->
          <div class="userAvatar">
            <img :src="user.avatar_url" alt />
          </div>
          <!-- userName -->
          <div class="title">
            <p>
              {{ user.name }}
            </p>
          </div>
          <!-- userReposQuantity -->
          <div class="userReposQuantity">
            <p>
             Repositories: {{ user.public_repos }}
            </p>
          </div>
        </div>
        <!-- wrapper -->
        <div class="repos__wrapper" v-if="repos">
          <!-- item -->
          <table class="table" v-if="user.public_repos > 0">
         <tr>
           <td @click="sort('name')">Name</td>
           <td @click="sort('star')">Star</td>
         </tr>
         </table>
          <div class="repos-item" v-for="repo in reposSort" :key="repo.id">
            <div class="repos-info">
              <a class="link" target="_blank" :href="repo.html_url">{{ repo.name }}</a>
              <span>{{ repo.stargazers_count }} ⭐</span>
            </div>
          </div>
        </div>
        <!-- <p>{{ search }}</p> -->
      </div>
    </section>
  </div>
</template>

<script>
import search from "@/components/Search.vue";
import axios from "axios";
export default {
  components: {
    search
  },
  data() {
    return {
      search: "",
      error: null,
      repos: null,
      user: null,
      currentSort: "name",
      currentSortDir: "asc",
    };
  },
  computed: {
    reposSort (){
      return this.repos.sort((a, b) => {
          let mod = 1;
          if (this.currentSortDir === "desc") mod = -1
          // // Name sort
          if(this.currentSort === 'name'){
          if (a[this.currentSort] < b[this.currentSort]) return -1 * mod;
          if (a[this.currentSort] > b[this.currentSort]) return 1 * mod;
          }
         if(this.currentSort === 'star')
          //star sort
          if (a.stargazers_count < b.stargazers_count)  return -1 * mod;
          if (a.stargazers_count > b.stargazers_count) return 1 * mod;

          return 0;
        })
    }
  },
  methods: {
    getUser() {
      axios
      .get(`https://api.github.com/users/${this.search}`)
        .then(res => {
          this.user = res.data;
      })
          .catch(err => {
            this.user = null;
            console.log(err);
            this.error = "Can`t find this repository";
          })

          // repos

            axios
          .get(`https://api.github.com/users/${this.search}/repos`)
            .then(res => {
               this.error = null;
                this.repos = res.data;
                console.log(this.repos);
            })
             .catch(err => {
          this.repos = null;
          console.log(err);
          this.error = "Can`t find this user";
        })
    },
    sort(e) {
      if(e == this.currentSort) {
        this.currentSortDir =  this.currentSortDir === 'asc' ? 'desc' : 'asc'
      } 
      this.currentSort = e
      console.log(e)
    }

  }
};
</script>
<style lang="scss" scoped>
.btn {

  &.btnPrimary {
  background-color: rgba(122,122,122,0.5);
  border-color: rgba(3, 3, 3, 0.89);
}
}

.userAvatar {
  margin-top: 30px;
  box-shadow: 0 0 5px 6px rgba(122,122,122,0.5);
}
.title {
  text-align-last: center;
  margin-top: 30px;
  text-shadow: 1px 1px 2px black, 0 0  1em rgba(122,122,122,0.5);
}
.container {
  display: flex;
  align-items: center;
  flex-direction: column;
}
.userReposQuantity {
  text-shadow: 1px 1px 2px black, 0 0  1em rgba(122,122,122,0.5); 
  text-align: center;
}
.repos__wrapper {
  width: 400 px;
  margin: 30px 0;
}
.repos-info {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 10px 0;
  border-bottom: 1px solid #dbdbdb;
}
.link {
  color: rgba(3, 3, 3, 0.89);
}
.name {
  position: relative;
  right: 100;
}
.star {
  position: relative;
  left: 100;
}

 td {
    padding: 0px  50px;
    white-space: nowrap;
    // font-size: px;
    cursor: pointer;
  }
</style>