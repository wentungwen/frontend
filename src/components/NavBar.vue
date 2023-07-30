<template>
  <div>
    <b-button v-b-toggle.sidebar-1>Toggle</b-button>
    <b-sidebar
      id="sidebar-1"
      title="Sidebar"
      shadow
      class="d-flex flex-column p-2 bg-light"
    >
      <b-col class="mb-3 flex-grow-1">
        <b-link
          href="/"
          class="d-flex align-items-center mb-3 mb-md-0 me-md-auto link-dark text-decoration-none"
        >
          <b-icon-bank></b-icon-bank>
          <span class="fs-4 pl-2">language learner</span>
        </b-link>
        <hr />
        <ul class="nav nav-pills flex-column mb-auto">
          <li
            class="nav-item"
            v-for="conversation in conversations"
            v-bind:key="conversation.id"
          >
            <a href="#" class="nav-link active mb-2" aria-current="page">
              <b-icon-x-circle-fill></b-icon-x-circle-fill>
              <span class="fs-4 pl-2">{{ conversation.title }}</span>
            </a>
          </li>
        </ul>
      </b-col>
      <b-col>
        <b-link
          href="#"
          class="d-flex align-items-center link-dark text-decoration-none"
          id="dropdownUser2"
        >
          <img
            src="https://github.com/mdo.png"
            alt=""
            width="32"
            height="32"
            class="rounded-circle mr-2"
          />
          <strong>mdo</strong>
        </b-link>
        <hr />
        <b-link href="#">Sign out</b-link>
      </b-col>
    </b-sidebar>
  </div>
</template>
<script>
export default {
  name: "NavBar",
  data() {
    return {};
  },
  props: {
    conversations: {
      type: Array,
    },
  },
};
</script>
<style>
.sidebar {
  height: 100vh;
  width: 200px;
}
</style>
