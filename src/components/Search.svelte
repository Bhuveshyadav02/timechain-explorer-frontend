<script>
// @ts-nocheck

  import axios from "axios";

  let items = [
    'All categories',
    'Block Height',
    'Block Hash',
    'Transcation',
    'Address'
  ];

  let selectCategory = 'All categories';
  let isDropdownOpen = false;
   let search=''
  function toggleDropdown() {
    isDropdownOpen = !isDropdownOpen;
  }
  /**
   * @param {string} term
   */
  function setSearch(term){
     search=term
   
  }

 /**
   * @param {string} input
   */
 function isBlockHash(input){
    return /^[0-9a-fA-F]{64}$/.test(input) && /^0{8,}/.test(input);
  }

  /**
   * @param {string} input
   */
  function isBlockHeight(input) {
    return /^\d+$/.test(input);
  }

  /**
   * @param {string} input
   */
  function isTxid(input){
    return /^[0-9a-fA-F]{64}$/.test(input) && !/^0{8,}/.test(input);
  }

  /**
   * @param {string} input
   */
  function isAddress(input) {
    return /^[1-9A-HJ-NP-Za-km-z]{26,35}$/.test(input);
  }

  async function handleSearch(){
    console.log(search,selectCategory)
     if(selectCategory===items[0]){
      console.log("Select Specific Search Term")
     }
     else if(selectCategory===items[4]&&isAddress(search)){
      console.log("This is Address")
     }
     else if(selectCategory===items[1]&&isBlockHash(search)){
      console.log("This is block Hash")
     }
     else if(selectCategory===items[2]&&isBlockHeight(search)){
      console.log("This is Block Height")
     }
     else if(selectCategory===items[3]&&isTxid(search)){
      console.log("This is transcrion")
     }
     
     try {
    const response = await axios.post('http://localhost:3000/api/search', {
      input: search,
      //type: searchType, // Include this if your backend needs it
    }, {
      headers: {
        'Content-Type': 'application/json',
      }
    });

    const searchResults = response.data;
    console.log(searchResults);
  } catch (error) {
    console.error('Axios error: ', error.response ? error.response.data : error.message);
  }

     

  }

  /**
   * @param {string} item
   */
  function selectItem(item) {
    selectCategory = item;
    isDropdownOpen = false; 
  }
</script>

<form class="flex items-center space-x-2">
  <div class="relative">
    <button
      type="button"
      on:click={toggleDropdown}
      class="flex items-center px-4 py-2 border border-primary-700 rounded-e-none bg-white text-gray-900"
    >
      {selectCategory}
      <svg class="w-2.5 h-2.5 ml-2" fill="none" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 10 6">
        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 4 4 4-4"/>
      </svg>
    </button>
    {#if isDropdownOpen}
      <div class="absolute right-0 mt-1 w-40 bg-white border border-gray-300 rounded-lg shadow-lg">
        {#each items as item}
          <button
            type="button"
            on:click={() => selectItem(item)}
            class="w-full text-left px-4 py-2 hover:bg-gray-100 focus:outline-none"
          >
            {item}
          </button>
        {/each}
      </div>
    {/if}
  </div>
  <input
    type="search"
    placeholder="Search Block hash/height, Transcation, Address..."
    class="w-full px-4 py-2 border border-gray-300 rounded-none"
  bind:value={search}/>
  <button
    type="submit"
    class="p-2.5 bg-blue-700 text-white rounded-s-none hover:bg-blue-800"
  on:click={handleSearch} >
    <svg class="w-6 h-6" fill="none" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
      <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"/>
    </svg>
  </button>
</form>

<style>
  /* Optional: Add styles for better visuals */
  .button-dropdown {
    transition: background-color 0.2s ease;
  }
  .button-dropdown:hover {
    background-color: #f0f0f0;
  }
</style>
