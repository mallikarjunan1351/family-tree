<template>
  <div id="app">
    <div class="text-center p-2">
      <h1 class="title">Family Tree</h1>

      <b-button
        class="m-2"
        @click="addPerson, (modalShow = !modalShow)"
        variant="success"
        >Add First Person</b-button
      >

      <b-button class="m-2" @click="remove" variant="danger"
        >Remove All</b-button
      >

      <b-button class="m-2" @click="load" variant="success"
        >Load Default</b-button
      >
    </div>

    <div>
      <b-modal class="m-1" v-model="modalShow" hide-footer title="Add Person">
        <div>
          <b-form @submit="onSubmit" @reset="onReset" v-if="show">
            <b-form-group label="Select Person Type">
              <b-form-radio
                class="m-1"
                v-model="form.type"
                name="radio-inline"
                value="First Person"
                >First Person</b-form-radio
              >
              <b-form-radio
                class="m-1"
                v-model="form.type"
                name="radio-inline"
                value="Second Person"
                >Second Person</b-form-radio
              >
              <b-form-radio
                class="m-1"
                v-model="form.type"
                name="radio-inline"
                value="Children"
                >Children</b-form-radio
              >
            </b-form-group>
            <b-form-group
              class="m-1"
              id="input-group-1"
              label="Name:"
              label-for="input-1"
            >
              <b-form-input
                id="input-1"
                v-model="form.name"
                type="text"
                placeholder="Enter Name"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group
              class="m-1"
              id="input-group-2"
              label="Age:"
              label-for="input-2"
            >
              <b-form-input
                id="input-2"
                type="number"
                min="0"
                max="200"
                v-model="form.age"
                placeholder="Enter Age"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group
              class="m-1"
              id="input-group-3"
              label="Gender:"
              label-for="input-3"
            >
              <b-form-select
                id="input-3"
                v-model="form.gender"
                :options="gender"
                required
              ></b-form-select>
            </b-form-group>

            <b-form-group
              class="m-1"
              id="input-group-4"
              label="Relationship:"
              label-for="input-4"
            >
              <b-form-select
                id="input-4"
                v-model="form.relationship"
                :options="relationship"
                required
              ></b-form-select>
            </b-form-group>

            <b-button class="m-1" type="reset" variant="danger">Reset</b-button>
            <b-button class="m-1" type="submit" variant="primary"
              >Submit</b-button
            >
          </b-form>
        </div>
      </b-modal>
    </div>

    <VueTreeList @treelist="updateTree"></VueTreeList>

    <VueFamilyTree :tree="tree" @card-click="cardClick">
      <template v-if="customCard" v-slot:card="{ item }">
        <div
          class="custom-card"
          v-bind:class="item.gender === 'male' ? 'male' : 'female'"
        >
          <div
            :style="{
              backgroundImage: item.image ? `url(${item.image})` : null,
            }"
            class="custom-card__image"
          />
          <div class="custom-card__info">
            <div class="custom-card__name">
              {{ item.name }}
            </div>
            <div v-show="item.age" class="custom-card__age">
              Age: {{ item.age }}
            </div>
            <div v-show="item.relationship" class="custom-card__age">
              {{ item.relationship }}
            </div>
          </div>
        </div>
      </template>
    </VueFamilyTree>
  </div>
</template>

<script>
import VueFamilyTree from "./components/VueFamilyTree";
import VueTreeList from "./components/VueTreeList";
export default {
  name: "App",
  components: {
    VueFamilyTree,
    VueTreeList,
  },
  data() {
    return {
      form: {
        name: "",
        age: "",
        gender: null,
        relationship: null,
        type: "",
      },
      gender: [{ text: "Select One", value: null }, "male", "female"],
      relationship: [
        { text: "Select One", value: null },
        "Father",
        "Mother",
        "Son",
        "Daughter",
      ],
      show: true,
      customCard: false,
      modalShow: false,
      tree: [
        {
          firstPerson: {
            nodeId: 0,
            level: 0,
            name: "John Walker",
            image: "https://picsum.photos/300/300?random=1",
            age: "60",
            relationship: "Father",
            gender: "male",
          },
          secondPerson: {
            nodeId: 1,
            level: 0,
            name: "Jannet Grem",
            image: "https://picsum.photos/300/300?random=2",
            age: "55",
            relationship: "Mother",
            gender: "female",
          },
          children: [
            {
              firstPerson: {
                nodeId: 2,
                level: 1,
                name: "Katia",
                image: "https://picsum.photos/300/300?random=3",
                age: "45",
                relationship: "Son",
                gender: "male",
              },
              secondPerson: {
                nodeId: 3,
                level: 1,
                name: "Oleg",
                image: "https://picsum.photos/300/300?random=4",
                age: "38",
                relationship: "Daughter In Law",
                gender: "female",
              },
              children: [
                {
                  firstPerson: {
                    nodeId: 4,
                    level: 2,
                    name: "Gleb",
                    image: "https://picsum.photos/300/300?random=5",
                    age: "23",
                    relationship: "Daughter",
                    gender: "female",
                  },
                  children: [],
                },
                {
                  firstPerson: {
                    nodeId: 5,
                    level: 2,
                    name: "Olga",
                    image: "https://picsum.photos/300/300?random=6",
                    age: "30",
                    relationship: "Son",
                    gender: "male",
                  },
                  secondPerson: {
                    nodeId: 6,
                    level: 2,
                    name: "Nikita",
                    image: "https://picsum.photos/300/300?random=7",
                    age: "23",
                    relationship: "Daughter In Law",
                    gender: "female",
                  },
                },
              ],
            },
            {
              firstPerson: {
                nodeId: 7,
                level: 1,
                name: "Vitia",
                image: "https://picsum.photos/300/300?random=7",
                age: "23",
                relationship: "Daughter",
                gender: "female",
              },
              children: [],
            },
            {
              firstPerson: {
                nodeId: 8,
                level: 1,
                name: "Antonio Wild",
                image: "https://picsum.photos/300/300?random=6",
                age: "30",
                relationship: "Son",
                gender: "male",
              },
              secondPerson: {
                nodeId: 9,
                level: 1,
                name: "Olivia Olson",
                image: "https://picsum.photos/300/300?random=7",
                age: "23",
                relationship: "Daughter In Law",
                gender: "female",
              },
              children: [
                {
                  firstPerson: {
                    nodeId: 10,
                    level: 2,
                    name: "Kristina Wild",
                    image: "https://picsum.photos/300/300?random=7",
                    age: "13",
                    relationship: "Daughter",
                    gender: "female",
                  },
                },
              ],
            },
          ],
        },
      ],
      result: {},
    };
  },
  methods: {
    cardClick(item) {
      console.log(item);
      this.modalShow = true;
    },
    updateTree(treelist) {
      console.log({ treelist });
      this.tree = treelist.children;
    },
    onSubmit(event) {
      event.preventDefault();
      console.log(this.form);
      this.addPerson();
      this.onReset(event);
      this.modalShow = false;
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.form.name = "";
      this.form.age = "";
      this.form.gender = null;
      this.form.relationship = null;
      this.form.type = "";
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
    addPerson() {
      console.log("Add");
      this.form["image"] = "https://picsum.photos/300/300?random=1";
      this.form["id"] = Math.random();
      let family = {};
      console.log(this.form);
      if (this.form.relationship == "Father") {
        family.firstPerson = this.form;
      } else {
        family.secondPerson = this.form;
      }
      console.log(JSON.stringify(this.tree));
      this.tree.push(family);
      console.log(JSON.stringify(this.tree));
      localStorage.setItem("tree", JSON.stringify(this.tree));
      this.tree = JSON.parse(localStorage.getItem("tree"));
    },
    remove(item) {
      console.log("Remove");
      localStorage.removeItem("tree");
      this.tree = [];
      if (!item) {
        localStorage.removeItem("tree");
        this.tree = [];
      } else {
        this.tree.forEach((element) => {
          console.log(element);
        });
        console.log("After Remove : " + JSON.stringify(this.tree));
      }
    },
    load() {
      localStorage.removeItem("tree");
      this.tree = [
        {
          firstPerson: {
            nodeId: 0,
            level: 0,
            name: "John Walker",
            image: "https://picsum.photos/300/300?random=1",
            age: "60",
            relationship: "Father",
            gender: "male",
          },
          secondPerson: {
            nodeId: 1,
            level: 0,
            name: "Jannet Grem",
            image: "https://picsum.photos/300/300?random=2",
            age: "55",
            relationship: "Mother",
            gender: "female",
          },
          children: [
            {
              firstPerson: {
                nodeId: 2,
                level: 1,
                name: "Katia",
                image: "https://picsum.photos/300/300?random=3",
                age: "45",
                relationship: "Son",
                gender: "male",
              },
              secondPerson: {
                nodeId: 3,
                level: 1,
                name: "Oleg",
                image: "https://picsum.photos/300/300?random=4",
                age: "38",
                relationship: "Daughter In Law",
                gender: "female",
              },
              children: [
                {
                  firstPerson: {
                    nodeId: 4,
                    level: 2,
                    name: "Gleb",
                    image: "https://picsum.photos/300/300?random=5",
                    age: "23",
                    relationship: "Daughter",
                    gender: "female",
                  },
                  children: [],
                },
                {
                  firstPerson: {
                    nodeId: 5,
                    level: 2,
                    name: "Olga",
                    image: "https://picsum.photos/300/300?random=6",
                    age: "30",
                    relationship: "Son",
                    gender: "male",
                  },
                  secondPerson: {
                    nodeId: 6,
                    level: 2,
                    name: "Nikita",
                    image: "https://picsum.photos/300/300?random=7",
                    age: "23",
                    relationship: "Daughter In Law",
                    gender: "female",
                  },
                },
              ],
            },
            {
              firstPerson: {
                nodeId: 7,
                level: 1,
                name: "Vitia",
                image: "https://picsum.photos/300/300?random=7",
                age: "23",
                relationship: "Daughter",
                gender: "female",
              },
              children: [],
            },
            {
              firstPerson: {
                nodeId: 8,
                level: 1,
                name: "Antonio Wild",
                image: "https://picsum.photos/300/300?random=6",
                age: "30",
                relationship: "Son",
                gender: "male",
              },
              secondPerson: {
                nodeId: 9,
                level: 1,
                name: "Olivia Olson",
                image: "https://picsum.photos/300/300?random=7",
                age: "23",
                relationship: "Daughter In Law",
                gender: "female",
              },
              children: [
                {
                  firstPerson: {
                    nodeId: 10,
                    level: 2,
                    name: "Kristina Wild",
                    image: "https://picsum.photos/300/300?random=7",
                    age: "13",
                    relationship: "Daughter",
                    gender: "female",
                  },
                },
              ],
            },
          ],
        },
      ];
    },
  },
  mounted() {
    this.tree =
      JSON.parse(localStorage.getItem("tree")) !== null
        ? JSON.parse(localStorage.getItem("tree"))
        : this.tree;
    console.log(`the component is now mounted.${JSON.stringify(this.tree)}`);
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

.text-center {
  text-align: center;
  margin-bottom: 32px;
}

.custom-card {
  display: flex;
  align-items: center;
  width: 220px;
  padding: 16px;
  border: 1px solid #ddd;
  border-radius: 10px;
  box-sizing: border-box;
  background-color: #fff;
  cursor: pointer;
  box-shadow: 0 0 6px 2px rgba(#000, 0);
  transition: box-shadow 0.2s ease;
  &:hover {
    box-shadow: 0 0 6px 2px rgba(#000, 0.1);
  }
  &__image {
    flex: 0 0 auto;
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background-color: #dedede;
    background-size: cover;
    background-position: 50%;
  }
  &__info {
    padding-left: 16px;
  }
  &__name {
    font-weight: 600;
  }
  &__age {
    margin-top: 4px;
    font-size: 12px;
  }
}

.female {
  background: #fdaed8;
}

.male {
  background: #a4ecff;
}
</style>
