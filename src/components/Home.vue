<template>
  <div class="Home">
    <div class="container">
      <div class="row">
        <div class="col-12 col-md-6">
          <div class="block-wrapper">
            <h2>Users:</h2>
            <div v-if="users.length">
              <div class="search d-flex flex-column flex-md-row align-items-center">
                <span>Search:</span>
                <input class="search-input" type="text" v-model="searchUsers">
              </div>
              <div class="table">
                  <ul class="table-header d-flex">
                    <li class="flex-1 d-flex justify-content-center">
                      <p class="table-header__title">Name</p>
                    </li>
                    <li class="flex-2 d-flex justify-content-center">
                      <p class="table-header__title">Groups</p>
                    </li>
                    <li class="flex-1 d-flex justify-content-center">
                      <p class="table-header__title">User Options</p>
                    </li>
                  </ul>
                  <div class="table-content">
                    <ul class="table-line d-flex" v-for="(user, index) in filterUsers" :key="user.id">
                      <li class="flex-1 d-flex justify-content-center">
                        <p class="table-name">{{user.name}}</p>
                      </li>
                      <li class="flex-2 d-flex">
                        <div v-if="user.groups.length" class="d-flex justify-content-around groups-wrapper">
                          <div v-for="(group, index) in getUserGroups(user.groups)" :key="group.id" class="d-flex flex-column flex-lg-row align-items-center">
                            <span class="table-group" v-text="group.name"></span>
                            <button class="btn btn-remove__group" type="button" @click="leaveGroup(user.groups, index)">
                              <svg x="0px" y="0px" width="15px" height="15px" viewBox="0 0 512 512" style="enable-background:new 0 0 512 512;" xml:space="preserve">
                                <circle style="fill:#E21B1B;" cx="256" cy="256" r="256"/>
                                <path style="fill:#C40606;" d="M510.28,285.304L367.912,142.936L150.248,368.608l140.928,140.928
                                  C406.352,493.696,497.056,401.288,510.28,285.304z"/>
                                <g>
                                  <path style="fill:#FFFFFF;" d="M354.376,371.536c-5.12,0-10.232-1.952-14.144-5.856L146.408,171.848
                                    c-7.816-7.816-7.816-20.472,0-28.28s20.472-7.816,28.28,0L368.52,337.4c7.816,7.816,7.816,20.472,0,28.28
                                    C364.608,369.584,359.496,371.536,354.376,371.536z"/>
                                  <path style="fill:#FFFFFF;" d="M160.544,371.536c-5.12,0-10.232-1.952-14.144-5.856c-7.816-7.816-7.816-20.472,0-28.28
                                    l193.832-193.832c7.816-7.816,20.472-7.816,28.28,0s7.816,20.472,0,28.28L174.688,365.68
                                    C170.784,369.584,165.664,371.536,160.544,371.536z"/>
                                </g>
                              </svg>
                            </button>
                          </div>
                        </div>
                      </li>
                      <li class="flex-1 d-flex justify-content-center">
                        <button class="table-line__remove" @click="removeUser(index)">
                          <span class="d-none d-lg-block">Remove</span>
                          <svg class="d-block d-lg-none" x="0px" y="0px" width="15px" height="15px" viewBox="0 0 512 512" style="enable-background:new 0 0 512 512;" xml:space="preserve">
                            <circle style="fill:#E21B1B;" cx="256" cy="256" r="256"/>
                            <path style="fill:#C40606;" d="M510.28,285.304L367.912,142.936L150.248,368.608l140.928,140.928
                              C406.352,493.696,497.056,401.288,510.28,285.304z"/>
                            <g>
                              <path style="fill:#FFFFFF;" d="M354.376,371.536c-5.12,0-10.232-1.952-14.144-5.856L146.408,171.848
                                c-7.816-7.816-7.816-20.472,0-28.28s20.472-7.816,28.28,0L368.52,337.4c7.816,7.816,7.816,20.472,0,28.28
                                C364.608,369.584,359.496,371.536,354.376,371.536z"/>
                              <path style="fill:#FFFFFF;" d="M160.544,371.536c-5.12,0-10.232-1.952-14.144-5.856c-7.816-7.816-7.816-20.472,0-28.28
                                l193.832-193.832c7.816-7.816,20.472-7.816,28.28,0s7.816,20.472,0,28.28L174.688,365.68
                                C170.784,369.584,165.664,371.536,160.544,371.536z"/>
                            </g>
                          </svg>
                        </button>
                      </li>
                    </ul>
                  </div>
              </div>
              <p :class="{'form-error__message': deleteGroup}" v-if="deleteGroup"> {{deleteGroupMessage}} </p>
            </div>
            
            <div class="create-form__wrapper">
              <p>Create Users here</p>
              <form @submit.prevent="createUser" class="create-form d-flex flex-column">
                <input type="text" placeholder="Please type user name" v-model="userForm.name" class="create-form__input" :class="{'form-error': userErrorMessage }">
                <div v-for="group in groups" :key="group.id" class="d-flex align-items-center">
                  <input class="create-form__checkbox" type="checkbox" :id="group.id" :value="group.id" v-model="userForm.groups">
                  <label class="create-form__label" :for="group.id" v-text="group.name"></label>
                </div>
                <button class="btn btn-form">Create</button>
              </form>
              <p class="form-error__message" v-show="userErrorMessage">{{ formErrorMessage }}</p>
            </div>
          </div>
        </div>
        <div class="col-12 col-md-6">
          <div class="block-wrapper">
            <h2>Groups:</h2>
            <div v-if="groups.length">
              <div class="search d-flex flex-column flex-md-row align-items-center">
                <span>Search:</span>
                <input class="search-input" type="text" v-model="searchGroups">
              </div>
              <div class="table">
                  <ul class="table-header d-flex">
                    <li class="flex-1 d-flex justify-content-center">
                      <p class="table-header__title">Group</p>
                    </li>
                    <li class="flex-1 d-flex justify-content-center">
                      <p class="table-header__title">Options</p>
                    </li>
                  </ul>
                  <div class="table-content">
                    <ul class="table-line d-flex" v-for="group in filterGroups" :key="group.id">
                      <li class="flex-1 d-flex justify-content-center">
                        <p class="table-name">{{group.name}}</p>
                      </li>
                      <li class="flex-1 d-flex justify-content-center">
                        <button class="table-line__remove" @click="removeGroup(group)">
                          <span class="d-none d-lg-block">Remove</span>
                          <svg class="d-block d-lg-none" x="0px" y="0px" width="15px" height="15px" viewBox="0 0 512 512" style="enable-background:new 0 0 512 512;" xml:space="preserve">
                            <circle style="fill:#E21B1B;" cx="256" cy="256" r="256"/>
                            <path style="fill:#C40606;" d="M510.28,285.304L367.912,142.936L150.248,368.608l140.928,140.928
                              C406.352,493.696,497.056,401.288,510.28,285.304z"/>
                            <g>
                              <path style="fill:#FFFFFF;" d="M354.376,371.536c-5.12,0-10.232-1.952-14.144-5.856L146.408,171.848
                                c-7.816-7.816-7.816-20.472,0-28.28s20.472-7.816,28.28,0L368.52,337.4c7.816,7.816,7.816,20.472,0,28.28
                                C364.608,369.584,359.496,371.536,354.376,371.536z"/>
                              <path style="fill:#FFFFFF;" d="M160.544,371.536c-5.12,0-10.232-1.952-14.144-5.856c-7.816-7.816-7.816-20.472,0-28.28
                                l193.832-193.832c7.816-7.816,20.472-7.816,28.28,0s7.816,20.472,0,28.28L174.688,365.68
                                C170.784,369.584,165.664,371.536,160.544,371.536z"/>
                            </g>
                          </svg>
                        </button>
                      </li>
                    </ul>
                  </div>
              </div>
            </div>
            <div class="create-form__wrapper">
              <p>Create Groups here</p>
              <form @submit.prevent="createGroup" class="create-form d-flex flex-column">
                <input type="text" placeholder="Please type the name of the group" class="create-form__input" :class="{'form-error': groupErrorMessage }" v-model="groupForm.name">
                <button class="btn btn-form">Create</button>
              </form>
              <p class="form-error__message" v-show="groupErrorMessage">{{ formErrorMessage }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { setTimeout } from 'timers';
export default {
  name: 'Home',
    data() {
    return {
      users: [],
      userForm: {
        name: "",
        groups: [],
      },
      formErrorMessage : "",
      userErrorMessage: false,
      groupErrorMessage: false,
      groups: [],
      groupForm: { name: "" },
      deleteGroup: false,
      deleteGroupMessage: "",
      searchGroups: "",
      searchUsers: ""
    };
  },
  created() {
    this.getItemsInStorage()
  },
  methods: {
    createUser() {
      if (this.groups.length < 1) {
        this.userErrorMessage = true;
        this.formErrorMessage = 'Please create a group first'
        this.disableUserErrorMessage();
      } else if (this.userForm.groups == '' && this.userForm.name == '') {
        this.userErrorMessage = true
        this.formErrorMessage = "Fill whole form"
        this.disableUserErrorMessage();
      } else if (this.userForm.groups == '' && this.userForm.name !== '') {
        this.userErrorMessage = true
        this.formErrorMessage = 'Select at least one group'
        this.disableUserErrorMessage();
      } else if (this.userForm.name == ''  && this.userForm.groups !== '') {
        this.userErrorMessage = true
        this.formErrorMessage = "Name can't be blank"
        this.disableUserErrorMessage();
      } else {
        this.userErrorMessage = false
        this.users.push({ id: new Date().getTime(), ...this.userForm });
        this.userForm = { name: "", groups: [] };
        localStorage.setItem('users', JSON.stringify(this.users));
      }
    },

    disableUserErrorMessage() {
      setTimeout(() => {
        this.userErrorMessage = false
      }, 2000); 
    },

    createGroup() {
      if (this.groupForm.name == '') {
        this.groupErrorMessage = true;
        this.formErrorMessage = "Name can't be blank"

        setTimeout(() => {
          this.groupErrorMessage = false
        }, 2000); 
        
      } else {
        this.groups.push({ id: new Date().getTime(), ...this.groupForm });
        this.groupForm = { name: "" };
        localStorage.setItem('groups', JSON.stringify(this.groups))
      }
    },

    getUserGroups(groupIds) {
      return this.groups.filter(({ id }) => groupIds.includes(id));
    },

    leaveGroup(groups, index) {
      if (groups.length < 2) {
        this.deleteGroup = true
        this.deleteGroupMessage = "User must have at least one group";
        setTimeout(() => {
          this.deleteGroup = false
        }, 2000)
      } else {
        groups.splice(index, 1);
        localStorage.setItem('users', JSON.stringify(this.users));
      }
    },

    removeUser(index) {
      this.users.splice(index, 1);
      localStorage.setItem('users', JSON.stringify(this.users));
    },

    removeGroup(group) {
      const hasMember = this.users.some(user => user.groups.includes(group.id));

      if (hasMember) {
        return alert("This group still has members!");
      }

      this.groups = this.groups.filter(grp => grp.id !== group.id);
      localStorage.setItem('groups', this.groups);
    },

    getItemsInStorage() {
      if (localStorage.users) {
        this.users = JSON.parse(localStorage.users);
      }
      if (localStorage.groups) {
        this.groups = JSON.parse(localStorage.groups);
      }
    }
  },
  computed: {
    filterGroups() {
      return this.groups.filter(group => {
        return group.name.includes(this.searchGroups)
      })
    },
    filterUsers() {
      return this.users.filter(user => {
        return user.name.includes(this.searchUsers)
      })
    }
  }
}
</script>


<style lang="scss" scoped>
@import '../assets/mixins.scss';

.flex-1 {
  flex: 1;
}

.flex-2 {
  flex: 2;
}

.table {
  border: 1px solid $grey;

  &-header, &-line {
    margin: 0;
    padding: 10px;
    list-style: none;
  }

  &-header {
    background: rgba($grey, 0.6);

    &__title {
      margin: 0;
      font-size: 14px;
      font-weight: bold;
      color: $black;
    }
  }

  &-line {
    border-bottom: 1px solid $grey;

    &:last-child {
      border-bottom: none;
    }

    &__remove {
      border: none;
      font-size: 14px;
      color: $red;
    }
  }

  &-group {
    font-size: 12px;
    text-align: center;
  }

  &-name {
    font-weight: bold;
    font-size: 14px;
    margin: 0;
  }
}

.btn {
  &-remove__group {
    border: none;
    padding: 0;
    margin-left: 5px;
  }

  &-form {
    background: rgba($green, 0.9);
    border: 1px solid rgba($green, 0.9);
    border-radius: 0;
    color: $white;
    font-size: 14px;
    font-weight: 300;
    transition: all .3s ease-in-out;
    margin-top: 10px;

    &:hover {
      background: $white;
      color: rgba($green, 0.9);
    }
  }
}

.groups-wrapper {
  width: 100%;
}

.form-error {
  border: 1px solid $red;
}

.form-error__message {
  color: $red;
}

.create-form {
  &__input {
    padding: 10px;
    font-size: 14px;
    width: 100%;
    color: $grey;

    &:focus {
      outline: none;
    }
  }

  &__checkbox {
    margin-right: 5px;
  }

  &__label {
    margin: 0;
    font-size: 14px;
    color: $black;
  }
}

.search {
  margin: 30px 0;
  &-input {
    padding: 10px;
    font-size: 14px;
    width: 100%;
    color: #808080;
    margin-left: 10px;
  }
}

.block-wrapper {
  margin-top: 25px;
}

@media (min-width: 992px) {
  .table {
    &-group, &-name {
      font-size: 16px;
    }
  }
}

</style>
