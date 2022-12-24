<script>
  let userinput = ""
  let apikey = import.meta.env.VITE_API_KEY
  let resultdata = []


async function showcode(){
  const res = await fetch("https://api.openai.com/v1/completions",{
    method:"POST",
    headers:{
      'Content-Type':'application/json',
      'Authorization':`Bearer ${apikey}`
    },
    body:JSON.stringify({
      prompt:userinput,
      temperature:0.7,
      max_tokens:256,
      top_p:1,
      frequency_penalty:0,
      presence_penalty:0,
      model:"code-davinci-002"
    })
  })

  const data = await res.json()
  const response = data.choices[0].text
  resultdata = [...resultdata,{text:response,question:userinput}]
}
</script>

{#if resultdata.length > 0}
  <div class="result_data">
    {#each resultdata as r}
    <div>
      <h2 style="text-align: center;">Result for : {r.question}</h2>

      <pre>
        {@html r.text}
      </pre>

    </div>
    {/each}
  </div>
{/if}

<!-- RENDER YOU INPUT AND BUTTON HERE -->

<div class="containerinput"> 
  <input type="text"
  placeholder="Type you Logic here ..." 
  class="inputtext" 
  bind:value={userinput}
  >
  <button 
  class="btnsend"
  on:click={showcode}

  >Show the code</button>
</div>

<!-- STYLING  -->
<style>
  .result_data{
    box-shadow: 0px 0px 15px rgba(0, 0, 0, 0.5);
    padding: 10px;
    background-color: #fffa7c;
    font-weight: bold;
    font-size: 15px;
    width: 100%;
    box-sizing: border-box;
  }
  .containerinput{
    display: flex;
    flex-direction: row;
    justify-content: center;
  }

  .inputtext{
    height: 60px;
    width: 60%;
    font-size: 30px;
    padding: 10px;
  }

  .btnsend{
    background-color: yellow;
    font-size: 20px;
    color: black;
  }
</style>