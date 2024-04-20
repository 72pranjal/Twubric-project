<!-- Please remove this file from your project -->
<template>
  <div class="twubric-section">
    <div class="title">
      <h3>Twubric</h3>
    </div>
    <div class="sorting-section">
      <h3>Sort By</h3>
      <div class="d-flex flex-row mb-3">
        <div class="action">
          <button @click="sortBy('total')">Twubric Score</button>
        </div>
        <div class="action">
          <button @click="sortBy('friends')">Friends</button>
        </div>
        <div class="action">
          <button @click="sortBy('influence')">Influence</button>
        </div>
        <div class="action add-right-border">
          <button @click="sortBy('chirpiness')">Chirpiness</button>
        </div>
      </div>
    </div>
    <div class="filter-section">
      <h3>Joined Twitter between</h3>
      <div class="d-flex flex-row mb-3">
        <div class="dated">
          <label for="startDate">Start Date</label>
          <input type="date" class="form-control" id="startDate" v-model="startDate">
        </div>
        <div class="dated">
          <label for="endDate">End Date</label>
          <input type="date" class="form-control" id="endDate" v-model="endDate">
        </div>
      </div>
    </div>
    <div v-if="filteredFollowers && this.filteredFollowers.length > 0" class="user-section">
      <div class="d-flex flex-row mb-3">
        <UserCard v-for="follower in filteredFollowers" 
         :key="follower.id" :follower="follower"
         @removeUser="userIdForRemove = $event" />
      </div>
    </div>
    <div v-else>
      <h4>"We couldn't find any users within the specified date."</h4>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      followers: [],
      startDate: null,
      endDate: null,
      sortByCriteria: null,
      twubricUsers: [],
      userIdForRemove: null
    };
  },
  computed: {
    filteredFollowers() {
      let filtered = this.twubricUsers;
      if (this.startDate && this.endDate) {
        filtered = filtered.filter(follower => {
          const joinDate = new Date(follower.join_date);
          return joinDate >= new Date(this.startDate) && joinDate <= new Date(this.endDate);
        });
      }
      if (this.sortByCriteria) {
        filtered = filtered.sort((a, b) => {
          return a.twubric[this.sortByCriteria] - b.twubric[this.sortByCriteria];
        });
      }
      if(this.userIdForRemove) {
        const filteredUser = filtered.filter(follower => follower.uid !== this.userIdForRemove);
        this.twubricUsers = filteredUser
        filtered = filteredUser;
      }
      return filtered;
    }
  },
  methods: {
    sortBy(criteria) {
      this.sortByCriteria = criteria
    }
  },
  async mounted() {
    try {
      const response = await this.$axios.$get(
        "https://gist.githubusercontent.com/pandemonia/21703a6a303e0487a73b2610c8db41ab/raw/82e3ef99cde5b6e313922a5ccce7f38e17f790ac/twubric.json"
      );
      if(response) {
        this.twubricUsers = response
      }
    } catch (error) {
      console.error("Error fetching followers:", error);
    }
  }
};
</script>

<style scoped>
.twubric-section {
  width: 100%;
  max-width: 100%;
  padding: 3% 6%;
}

.twubric-section .title {
  text-align: center;
  text-transform: uppercase;
}

.sorting-section h3,
.filter-section h3 {
  font-weight: 600;
  font-size: 18px;
}

.user-section {
  width: 100%;
}

.sorting-section,
.filter-section {
  width: 98%;
}

.mb-3 {
  align-items: center;
  width: 100%;
  flex-wrap: wrap;
}

.sorting-section button {
  border: none;
  background: #fff;
  font-size: 16px;
  color: #000;
  font-weight: 600;
}

.sorting-section button:hover {
  opacity: .7;
}

.sorting-section .mb-3 .action {
  flex: 0 0 25%;
  padding: 10px 0;
  border: 2px solid #000;
  border-right: none;
}

.add-right-border {
  border-right: 2px solid #000 !important;
}

.filter-section .mb-3 .dated {
  flex: 0 0 50%;
}

.user-section .mb-3 {
  gap: 10px;
}

.form-control {
  padding: 12px;
  border: 2px solid #000;
  border-radius: 0;
}

#endDate {
  border-left: none !important;
}

.filter-section .dated label {
  font-size: 16px;
  font-weight: 500;
  margin-bottom: 7px;
}
</style>