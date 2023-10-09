<script>
import usersFilter from './components/usersFilter.vue'
import usersPagination from './components/usersPagination.vue'

export default {
   name: 'app',
   components: {
      usersFilter,
      usersPagination
   },
   data() {
      return {
         users: [
            {
               "id": 1,
               "name": "Clio Becker",
               "age": 36,
               "last_login": 1440200480
            },
            {
               "id": 2,
               "name": "Joelle Lester",
               "age": 32,
               "last_login": 1521971826
            },
            {
               "id": 3,
               "name": "Garrett Downs",
               "age": 28,
               "last_login": 1626481786
            },
            {
               "id": 4,
               "name": "Lars Cherry",
               "age": 46,
               "last_login": 1628647191
            },
            {
               "id": 5,
               "name": "Alyssa Bowen",
               "age": 33,
               "last_login": 1451031535
            },
            {
               "id": 6,
               "name": "Jennifer Castro",
               "age": 21,
               "last_login": 1571677551
            },
            {
               "id": 7,
               "name": "Xena Schwartz",
               "age": 19,
               "last_login": 1589436670
            },
            {
               "id": 8,
               "name": "Pamela Flores",
               "age": 49,
               "last_login": 1472048337
            },
            {
               "id": 9,
               "name": "Cassady Carpenter",
               "age": 28,
               "last_login": 1615024493
            },
            {
               "id": 10,
               "name": "Cullen Foreman",
               "age": 29,
               "last_login": 1644789562
            },
            {
               "id": 11,
               "name": "Justina Lang",
               "age": 35,
               "last_login": 1592379524
            },
            {
               "id": 12,
               "name": "Thaddeus Moon",
               "age": 49,
               "last_login": 1643645602
            },
            {
               "id": 13,
               "name": "Marshall Holder",
               "age": 20,
               "last_login": 1665977747
            },
            {
               "id": 14,
               "name": "Glenna Molina",
               "age": 38,
               "last_login": 1498851440
            },
            {
               "id": 15,
               "name": "Yardley Wiley",
               "age": 22,
               "last_login": 1634812342
            },
            {
               "id": 16,
               "name": "Lara Anthony",
               "age": 44,
               "last_login": 1575621560
            },
            {
               "id": 17,
               "name": "Warren Cole",
               "age": 30,
               "last_login": 1529553536
            },
            {
               "id": 18,
               "name": "Nathan Tran",
               "age": 47,
               "last_login": 1504451122
            },
            {
               "id": 19,
               "name": "Ashely Armstrong",
               "age": 45,
               "last_login": 1534281406
            },
            {
               "id": 20,
               "name": "Briar Manning",
               "age": 36,
               "last_login": 1509491729
            },
            {
               "id": 21,
               "name": "Ann Lester",
               "age": 49,
               "last_login": 1550586714
            },
            {
               "id": 22,
               "name": "Omar Tanner",
               "age": 42,
               "last_login": 1669205952
            },
            {
               "id": 23,
               "name": "Lucas Humphrey",
               "age": 40,
               "last_login": 1671540534
            },
            {
               "id": 24,
               "name": "Desiree York",
               "age": 21,
               "last_login": 1538878539
            },
            {
               "id": 25,
               "name": "Elaine Terry",
               "age": 41,
               "last_login": 1522927830
            }
         ],
         filter: '',
         pageSize: 5,
         currentPage: 0,
         filterVisibility: false,
         sortByAge: false,
         directSort: true
      }
   },
   computed: {
      usersPages() {
         // return this.users.reduce((acc, item) => {
         return this.filteredUsers.reduce((acc, item) => {
            if (acc[acc.length-1].length === this.pageSize) acc.push([]);
            acc[acc.length-1].push(item);
            return acc;
         }, [[]]);
      },
      sortedUsers() {
         let sortedUsers;
         let column = this.sortByAge ? 'age' : 'id';

         if (this.directSort) {
            sortedUsers = this.users.sort((a, b) => a[column] > b[column] ? 1 : -1);
         } else {
            sortedUsers = this.users.sort((a, b) => a[column] < b[column] ? 1 : -1);
         }
         
         return sortedUsers;
      },
      filteredUsers() {
         this.currentPage = 0;
         return this.sortedUsers.filter(item => item.name.toLowerCase().includes(this.filter.toLowerCase()));
      }
   },
   methods: {
      showHideFilter() {
         this.filterVisibility = !this.filterVisibility;
         this.filter = '';
      },
      sortUsers(col) {
         this.sortByAge = col === 'age' ? true : false;
         this.directSort = !this.directSort;
      },
      changePage(value) {
         if (value === '»' && this.currentPage < this.usersPages.length-1) {
            this.currentPage++;
         } else if (value === '«' && this.currentPage > 0) {
            this.currentPage--;
         } else if (value !== '»' || value !== '«') {
            if (!isNaN(value-1)) this.currentPage = (value - 1);
         }
      }
   }
}
</script>

<template>
   <div class="wrapper">
      <header class="header">
         <h1 class="title">Список пользователей</h1>
      </header>
      <table class="table">
         <thead>
            <tr>
               <th class="table__col-number">
                  <div 
                     @click="sortUsers('id')" 
                     :class="['table__icon table__icon-sort', {'table__icon-sort--reverse': directSort && !sortByAge}]"
                  ></div>
                  <span>#</span>
               </th>
               <th class="table__col-name">
                  <div @click="showHideFilter" class="table__icon table__icon-filter"></div>
                  <div v-show="filterVisibility" class="table__filter">
                     <usersFilter 
                        v-model="filter"
                        @hideFilter="showHideFilter" 
                        placeholderAttr="Введите имя"
                     />
                  </div>
                  <span>name</span>
               </th>
               <th class="table__col-age">
                  <div 
                     @click="sortUsers('age')" 
                     :class="['table__icon table__icon-sort', {'table__icon-sort--reverse': directSort && sortByAge}]"
                  ></div>
                  <span>age</span>
               </th>
               <th class="table__col-login">last login</th>
               <th class="table__col-actions">actions</th>
            </tr>
         </thead>
         <tbody>
            <tr v-for="person in usersPages[currentPage]" :key="person.id">
               <td>{{ person.id }}</td>
               <td>{{ person.name }}</td>
               <td>{{ person.age }}</td>
               <td>{{ new Date(person.last_login * 1000).toLocaleString() }}</td>
               <td>
                  <div class="btns">
                     <button type="button">write to user</button>
                     <button type="button">block user</button>
                  </div>
               </td>
            </tr>
         </tbody>
      </table>
      <usersPagination 
         :active="currentPage + 1" 
         :amountPages="usersPages.length"
         @clickPagination="changePage"
      />
   </div>
</template>

<style lang="scss">
.header {
   line-height: 1.5;
   max-height: 100vh;
   padding: 40px 0;
}

.title {
   text-align: center;
   font-weight: 700;
}

.table {
	width: 100%;
	border: none;
   margin: 0 auto 60px;
   text-align: left;
   border-collapse: collapse;

   thead th {
      padding: 1.2em 1em;
      background: var(--color-purple);
      border: none;
      border-left: 1px solid var(--color-purple-dark);
      border-right: 1px solid var(--color-purple-dark);
      font-weight: bold;
      text-transform: uppercase;
      vertical-align: middle;
      position: relative;

      span {
         font-weight: bold;
      }
   }

   tbody td {
      border-left: 1px solid var(--color-purple-dark);
      border-right: 1px solid var(--color-purple-dark);
      padding: 0.7em 1em;
      vertical-align: top;
      vertical-align: middle;
   }

   thead tr th:first-child, 
   tbody tr td:first-child {
      border-left: none;
   }

   thead tr th:last-child, 
   tbody tr td:last-child {
      border-right: none;
   }

   tbody tr:nth-child(even){
      background: var(--color-purple-light);
   }

   tbody td button {
      min-width: 140px;
      display: block;
      padding: 0.2em 0.4em;
      background: gray;
      border: 1px solid var(--color-purple-dark);
      border-radius: 8px;
      margin-bottom: 0.2em;
      color: var(--color-purple-dark);
      font-weight: 700;
      transition: 0.3s;
      
      &:last-child {
         margin-bottom: 0;
      }
      
      &:hover {
         transform: scale(1.05);
         background: lightslategray;
      }
   }

   &__filter {
      width: 94%;
      position: absolute;
      top: 50%;
      left: 3%;
      transform: translateY(-50%);
   }

   &__icon {
      position: absolute;
      top: 50%;
      right: 5%;
      transform: translateY(-50%);
      transition: 0.3s;
      cursor: pointer;

      &:hover {
         filter: blur(2px);
      }

      &-filter {
         width: 1em;
         height: 1em;
         background: url('./assets/filter-icon.png') center / contain no-repeat;
      }

      &-sort {
         border: 0.5em solid transparent;
         border-top: 0.8em solid gray;
         border-bottom: none;

         &--reverse {
            transform-origin: 50% 20%;
            transform: rotate(180deg);
         }
      }
   }
}
</style>
