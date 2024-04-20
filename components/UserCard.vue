<template>
  <div class="user-card">
    <div class="name-section">
      <p>{{ follower.fullname }}</p>
      <p>{{ follower.twubric.total }}</p>
    </div>
    <div class="detail-section">
      <div class="details">
        <span>{{ follower.twubric.friends }}</span>
        <span>Friends</span>
      </div>
      <div class="details">
        <span>{{ follower.twubric.influence }}</span>
        <span>Influence</span>
      </div>
      <div class="details border-dispatch">
        <span>{{ follower.twubric.chirpiness }}</span>
        <span>Chirpiness</span>
      </div>
    </div>

    <div class="action-section">
      <div class="date">
        <span>{{ changeTimestampToDate }}</span>
      </div>
      <div class="remove-data">
        <button @click="removeFollower(follower.uid)">Remove</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "UserCard",
  props: ["follower"],
  computed: {
    changeTimestampToDate() {
      let date = new Date(this.follower.join_date);
      const months = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec",
      ];
      const month = months[date.getMonth()];
      const day = date.getDate();
      const year = date.getFullYear();
      return `${month} ${day}, ${year}`;
    },
  },
  methods: {
    removeFollower(id) {
       this.$emit('removeUser', id)
    },
  },
};
</script>

<style scoped>
.user-card {
  flex: 0 0 32%;
  padding: 3px 0px;
  border: 1px solid #000;
}

.user-card p {
  margin-bottom: 0px;
  font-size: 15px;
  font-weight: 600;
}

.user-card span {
  display: block;
  font-weight: 400;
  font-size: 13px;
}

.name-section {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #000;
  padding: 0 10px;
}

.detail-section {
  display: flex;
  width: 100%;
}

.detail-section .details {
  text-align: center;
  flex: 0 0 33%;
  border-right: 1px solid #000;
}
.border-dispatch {
  border-right: none !important;
}
.remove-data button {
  border: none;
  background: none;
  font-size: 15px;
  font-weight: 600;
}
.action-section {
  display: flex;
  align-items: baseline;
  padding: 0 10px;
  border-top: 1px solid #000;
}
.action-section .date {
  flex: 0 0 33%;
}
.action-section .date span {
  font-weight: 600 !important;
}
.action-section .remove-data {
  flex: 0 0 69%;
  text-align: end;
}
</style>
