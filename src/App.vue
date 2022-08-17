<template>
  <main>
    <button class="btn"></button>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Nombre</th>
          <th scope="col">Url</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(drink, index) in drinks" :key="drink.idDrink">
          <td>{{ index }}</td>
          <td>{{ drink.strDrink }}</td>
          <td><img :src="drink.strDrinkThumb" style="width: 150px" /></td>
          <td><button type="button" class="btn btn-primary">Detail</button></td>
        </tr>
      </tbody>
    </table>

    <!-- Button trigger modal -->
    <button
      type="button"
      class="btn btn-primary"
      data-toggle="modal"
      data-target="#exampleModal"
    >
      Launch demo modal
    </button>

    <!-- Modal -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">...</div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-dismiss="modal"
            >
              Close
            </button>
            <button type="button" class="btn btn-primary">Save changes</button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import { Modal } from 'bootstrap';

export default {
    component: {
    Modal
    },
  data() {
    return {
      drinks: [],
    };
  },

  async created() {
    try {
      let response = await fetch(
        "https://www.thecocktaildb.com/api/json/v1/1/search.php?f=a"
      );
      let body = await response.json();
      console.log(body.drinks);
      this.drinks = body.drinks;
    } catch (e) {
      console.error(e);
    }
  },
};
</script>
