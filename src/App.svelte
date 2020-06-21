<script>
  import { v4 } from "uuid";
  import Noty from "noty";
  import 'noty/lib/noty.css';
  import 'noty/lib/themes/nest.css'

  let products = [
    {
      id: 1,
      name: "HP Pavilion",
      description: "Hp Computer Basic",
      category: "Laptops"
    },
    {
      id: 2,
      name: "Keyboard",
      description: "Razer Teclado",
      category: "Pheripherials",
      imageURL:
        "https://images-na.ssl-images-amazon.com/images/I/81NbkJz9M-L._AC_SX466_.jpg"
    }
  ];

  let product = {
    id: "",
    name: "",
    description: "",
    category: "",
    imageURL: ""
  };

  const cleanProductForm = () => {
    product = {
      id: "",
      name: "",
      description: "",
      category: ""
    };
  };

  const addProduct = () => {
    //console.log(product);
    const newProduct = {
      id: v4(), //products.length+1, //Asignar autoincrementable con uuid
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL
    };
    products = products.concat(newProduct);
    console.log(products);
    //Limpiar Inputs
	cleanProductForm();
	new Noty({
		theme: 'nest',
		type: 'success',
		timeout: 3000,
		text: 'Product Added Successfully'
	}).show()
  };

  const updatedProduct = () => {
    //Comenzar a vaciar el nuevo producto
    let updatedProduct = {
      id: product.id,
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL
    };
    const productIndex = products.findIndex(p => p.id === product.id); //Encontrar la posicion del siguiente producto
    products[productIndex] = updatedProduct; //Sustituir valores en el arreglo
    console.log(productIndex);
    // Limpiar inputs y regresar valor a AGREGAR
    cleanProductForm();
	editStatus = false;
	new Noty({
		theme: 'nest',
		type: 'success',
		timeout: 3000,
		text: 'Product Updated Successfully'
	}).show()
  };

  const onSubmitHandler = e => {
    //e.preventDefault();	//No refrescar la pagina  SVELETE desde Form
    if (!editStatus) {
      addProduct();
    } else {
      updatedProduct();
    }
  };

  const deleteProduct = id => {
    //console.log(id)
	products = products.filter(product => product.id !== id); //Se agregen todos los productos MENOS del id que le pase , quita ese id del arreglo
	new Noty({
		theme: 'nest',
		type: 'success',
		timeout: 3000,
		text: 'Product Deleted Successfully'
	}).show()
  };

  let editStatus = false;

  const editProduct = productEdited => {
    product = productEdited;
    editStatus = true;
  };
</script>

<style>

</style>

<main>

  <div class="container p-4">
    <div class="row">
      <!--List Products-->
      <div class="col-md-6">
        {#each products as product}
          <div class="card mt-2">
            <div class="row">
              <div class="col-md-4">
                {#if product.imageURL}
                  <img src={product.imageURL} alt="" class="img-fluid p-2" />
                {:else}
                  <img src="img/no_product.jpg" alt="" class="img-fluid p-2" />
                {/if}
              </div>
              <div class="col-md-8">
                <li class="card-body">
                  <div class="card-header">{product.name}</div>
                  <div class="card-body">
                    <strong>{product.description}</strong>
                    <hr />
                    <span>
                      <small>{product.category}</small>
                    </span>
                  </div>
                  <button
                    on:click={deleteProduct(product.id)}
                    class="btn btn-danger">
                    Delete
                  </button>
                  <button on:click={editProduct(product)} class="btn btn-info ">
                    Edit
                  </button>
                </li>
              </div>
            </div>
          </div>
        {/each}
      </div>
      <!--Form Product-->
      <div class="col-md-6">
        <div class="card">
          <div class="card-header text-center">
            <h1>Product Form</h1>
          </div>
          <form on:submit|preventDefault={onSubmitHandler} class="card-body">
            <div class="form-group">

              <input
                bind:value={product.name}
                class="form-control"
                id="name"
                type="text"
                placeholder="Product Name" />
            </div>
            <div class="form-group">
              <textarea
                bind:value={product.description}
                class="form-control"
                id="description"
                rows="3"
                placeholder="Product Description" />
            </div>
            <div class="form-group">
              <select
                bind:value={product.category}
                id="category"
                class="form-control">
                <option value="Laptops">Laptops</option>
                <option value="Pheriphelials">Pheriphelials</option>
                <option value="Servers">Servers</option>
              </select>
            </div>
            <div class="form-group">
              <input
                bind:value={product.imageURL}
                class="form-control"
                type="url"
                id="img-url"
                placeholder="https://exampleimage.png" />
            </div>
            <button class="btn btn-success btn-block">
              {#if !editStatus}SaveProduct{:else}UpdateProduct{/if}
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>

</main>
