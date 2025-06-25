async function searchPokemon() {
  
  const input = document.getElementById("pokemon-input").value.toLowerCase().trim();
  const resultDiv = document.getElementById("result");
  resultDiv.innerHTML = ""; // Clear previous result use ID from 1 to 20

  if (!input) {
    resultDiv.innerHTML = "<p class='error'>Please enter a Pokemon name or ID.</p>";
    return;
  }

  try {
    const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${input}`);
    if (!response.ok) throw new Error("Pokemon not in the database");

    const data = await response.json();
    const name = data.name.charAt(0).toUpperCase() + data.name.slice(1);
    const image = data.sprites.front_default;
    const type = data.types.map(t => t.type.name).join(", ");

    resultDiv.innerHTML = `
      <h2>${name}</h2>
      <img src="${image}" alt="${name}" />
      <p><strong>Type:</strong> ${type}</p>
    `;
  } catch (error) {
    resultDiv.innerHTML = "<p class='error'>Pokemon not in the database. Please try again.</p>";
  }
}
