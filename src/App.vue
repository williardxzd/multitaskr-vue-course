<template>
  <main>
    <!-- Modal -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">{{ drinkTitle }}</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <img :src="image" style="width: 100%" />
            <p>{{ instructions }}</p>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Close
            </button>
          </div>
        </div>
      </div>
    </div>

    <button class="btn"></button>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Name</th>
          <th scope="col">Image</th>
          <th scope="col">Instructions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(drink, index) in drinks" :key="drink.idDrink">
          <td>{{ index }}</td>
          <td>{{ drink.strDrink }}</td>
          <td><img :src="drink.strDrinkThumb" style="width: 150px" /></td>
          <td>
            <!-- Button trigger modal -->
            <button
              type="button"
              class="btn btn-primary"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
              @click="
                loadDetail(
                  drink.strDrink,
                  drink.strDrinkThumb,
                  drink.strInstructions
                )
              "
            >
              Instructions
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </main>
</template>

<script>
export default {
  data() {
    return {
      drinks: [],
      drinkTitle: "",
      instructions: "",
      image: "",
    };
  },

  async created() {
    try {
      let response = await fetch(
        "https://www.thecocktaildb.com/api/json/v1/1/search.php?f=a"
      );
      let body = await response.json();
      this.drinks = body.drinks;
    } catch (e) {
      console.error(e);
    }
  },

  methods: {
    loadDetail(drink, image, instructions) {
      this.drinkTitle = drink;
      //console.log(drink)
      this.image = image;
      //console.log(image)
      this.instructions = instructions;
      //console.log(instructions)
    },
  },
};
</script>
