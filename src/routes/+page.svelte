<script lang="ts">
  import {
    Navbar,
    NavBrand,
    NavLi,
    NavUl,
    NavHamburger,
    ImagePlaceholder,
    Skeleton,
    TextPlaceholder,
    Card,
    Gallery,
  } from "flowbite-svelte";
  import { onMount } from "svelte";

  let items: any = [];
  let newItemTitle = "";
  let newItemContent = "";
  let selectedItem: any = null; // Track the selected item for editing

  onMount(async () => {
    // Fetch data from the REST API when the component is mounted
    await fetchData();
  });

  let fetchData = async () => {
    try {
      const response = await fetch("http://localhost:3000/notes");
      const data = await response.json();
      items = data;
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  };

  let deleteData = async (id: any) => {
    try {
      const response = await fetch(`http://localhost:3000/notes/${id}`, {
        method: "DELETE",
      });

      if (response.ok) {
        console.log(`Item with ID ${id} deleted successfully`);
        // Update your local state to remove the deleted item
        items = items.filter((item: any) => item.id !== id);
      } else {
        console.error(`Failed to delete item with ID ${id}`);
      }
    } catch (error) {
      console.error("Error deleting data:", error);
    }
  };

  let addData = async () => {
    try {
      const response = await fetch("http://localhost:3000/notes", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          title: newItemTitle,
          content: newItemContent,
        }),
      });

      if (response.ok) {
        console.log("Item added successfully");
        // Fetch updated data after adding a new item
        await fetchData();
        // Clear input fields
        newItemTitle = "";
        newItemContent = "";
      } else {
        console.error("Failed to add item");
      }
    } catch (error) {
      console.error("Error adding data:", error);
    }
  };

  // Function to update an existing item
  let updateData = async () => {
    if (!selectedItem) {
      console.error("No item selected for update");
      return;
    }

    try {
      const response = await fetch(
        `http://localhost:3000/notes/${selectedItem.id}`,
        {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            id: selectedItem.id,
            title: newItemTitle, // Update with the new title
            content: newItemContent, // Update with the new content
          }),
        }
      );

      if (response.ok) {
        console.log(`Item with ID ${selectedItem.id} updated successfully`);
        // Fetch the updated list of items after updating
        await fetchData();
        // Clear input fields and reset the selected item
        newItemTitle = "";
        newItemContent = "";
        selectedItem = null;
      } else {
        console.error(`Failed to update item with ID ${selectedItem.id}`);
      }
    } catch (error) {
      console.error("Error updating data:", error);
    }
  };

  // Function to select an item for update
  let selectItemForUpdate = (item: any) => {
    selectedItem = item;
    newItemTitle = item.title;
    newItemContent = item.content;
  };
</script>

<!-- <h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p> -->

<div class="relative px-8">
  <main class="mt-24 mx-auto max-w-2xl grid grid-cols-3 gap-4">
    <h1 class="col-span-3 text-3xl font-bold mb-4">Notes REST API Frontend</h1>

    <!-- Input fields for adding or updating an item -->
    <div class="col-span-3 mb-4">
      <label for="newItemTitle" class="block text-sm font-medium text-gray-700"
        >Title:</label
      >
      <input
        type="text"
        id="newItemTitle"
        bind:value={newItemTitle}
        class="mt-1 p-2 border rounded-md w-full"
      />

      <label
        for="newItemContent"
        class="block mt-4 text-sm font-medium text-gray-700">Content:</label
      >
      <textarea
        id="newItemContent"
        bind:value={newItemContent}
        class="mt-1 p-2 border rounded-md w-full"
      />

      {#if selectedItem}
        <!-- Show update button when an item is selected -->
        <button
          on:click={updateData}
          class="mt-4 bg-blue-500 text-white px-4 py-2 rounded"
          >Update Item</button
        >
      {:else}
        <!-- Show add button when no item is selected -->
        <button
          on:click={addData}
          class="mt-4 bg-green-500 text-white px-4 py-2 rounded"
          >Add Item</button
        >
      {/if}
    </div>

    <ul class="col-span-3">
      {#each items as item}
        <li class="mb-2 p-4 bg-white rounded shadow">
          <div class="font-bold">
            {item.title}
          </div>
          <div>
            {item.content}
          </div>
          <button
            on:click={() => deleteData(item.id)}
            class="bg-red-500 text-white px-4 py-2 rounded">Delete</button
          >
          <button
            on:click={() => selectItemForUpdate(item)}
            class="mt-2 bg-blue-500 text-white px-4 py-2 rounded">Edit</button
          >
        </li>
      {/each}
    </ul>
  </main>
</div>
