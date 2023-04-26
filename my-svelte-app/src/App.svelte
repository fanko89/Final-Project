<script>
	import './lib/styles/styles.css';
	import { onMount } from 'svelte';
   
	import html2canvas from 'html2canvas';

	

	let yourTotal = 0;
	let yourAmountDue = 0;
	let inputHeight = 'auto';
	let paidElement = null;
  
	
	function getPDF() {
	  html2canvas(document.querySelector("#pdfWrap"), {scrollY: -window.scrollY}).then(function (canvas) {
		let pdfWidth = 208;
		let pdfHeight = canvas.height * pdfWidth / canvas.width;
		
		const contentDataURL = canvas.toDataURL('image/png')
		let pdf = new jsPDF('p', 'mm', [pdfWidth, pdfHeight]);
		pdf.addImage(contentDataURL, 'PNG', 0, 0, pdfWidth, pdfHeight)
		
		pdf.autoTable({ html: '#items', startY: pdfHeight - 100 });
		
		pdf.save('Invoice.pdf');
	  });
	}
	

	
	function resizeInput() {
	  const textarea = event.target;
	  textarea.style.height = 'auto';
	  textarea.style.height = `${textarea.scrollHeight}px`;
	  inputHeight = `${textarea.scrollHeight}px`;
	}
  
  
  
  
	
	function getCurrentDate() {
	  const today = new Date();
	  const dd = String(today.getDate()).padStart(2, '0');
	  const mm = String(today.getMonth() + 1).padStart(2, '0');
	  const yyyy = today.getFullYear();
	  return mm + '/' + dd + '/' + yyyy;
	}
  
	onMount(async() => {
	  const randomNumber = Math.floor(Math.random() * 1000000000)
	  document.getElementById('randomNumber').textContent = randomNumber
	  paidElement = document.querySelector('#paid');
	  updateTotal();
	});
	
	let rows = [{name: '', description: '', cost: 0, qty: 0, price: 0}]
  
	function updatePrice(row) {
	  let price = row.cost * row.qty;
	  row.price = isNaN(price) ? 'N/A' : price.toFixed(2)
	  console.log(row.price)
	  updateTotal();
	}
  
	function updateTotal() {
	  let total = rows.reduce((acc, row) => acc + Number(row.price), 0)
	  yourTotal = total;
	  updateBalance();
	  if (paidElement) {
		yourAmountDue = yourTotal - Number(paidElement.value.replace('$', ''))
	  } else {
		yourAmountDue = yourTotal;
	  }
	}
  
	function updateBalance() {
	  const balanceElement = document.querySelector("#balance");
	  if (balanceElement) {
		balanceElement.innerHTML = calculateBalance().toFixed(2);
	  }
	}
  
	function addRow() {
	  const index = rows.length;
	  rows = [...rows, { index, name: '', description: '', cost: 0, qty: 0, price: 0 }];
	}
  
	function removeRow(index) {
	  rows = rows.filter((row) => row.index !== index);
	  updateTotal();
	}
  </script>

<main>
	<form class="ui form">
	  <div id="pdfWrap">
		<textarea id="header">INVOICE</textarea>
		<div id="info">
		  <p><strong>155, UT 84443 | (801) 123 1234 | Fax: (801) 123 1234| website.com | office@website.com</strong></p>
		</div>
		<div class="logo">
		  <img id="image" src="/img/logo.png" alt="logo" />
		</div>
		<div id="identity">
		  <h3>Customers Info:</h3>
		
		
		  <input type="text" id="name" placeholder="Customer Name" on:input={resizeInput} />
		  <input type="text" id="address" placeholder="Street Address" on:input={resizeInput} />
		  <input type="text" id="region" placeholder="City, State Zip" on:input={resizeInput}/>
		  <input type="text" id="phone" placeholder="Phone Number" on:input={resizeInput} />
		  <input type="text" id="email" placeholder="Email Address" on:input={resizeInput} />
	
		</div>
		<div style="clear:both"></div>
		<div id="tec">
		  <h3>Technician Name:</h3>
		  <br />
		  <input type="text" id="tec-name" placeholder="Your Name"   on:input={resizeInput} />
		  <table id="meta">
			<tr>
			  <td class="meta-head">Invoice #</td>
			  <td> <p id="randomNumber"></p></td>
			</tr>
				  <tr>
					  <td class="meta-head">Date</td>
					  <td><input type="text" id="date"  placeholder={getCurrentDate()} /></td>
				  </tr>
				  <tr>
					<td class="meta-head">Amount Due</td>
					<td><div class="due">${yourAmountDue.toFixed(2)}</div></td>
				  </tr>
				</table>
			  </div>
			  <table id="items">
				<thead>
				  <tr>
					<th id="itemName">Item</th>
					<th>Description/Material</th>
					<th>Cost</th>
					<th>Quant</th>
					<th>Price</th>
				  </tr>
				</thead>
				<tbody>
					{#each rows as row, i (row.index)}
					<tr class="item-row" key={row.index}>
						<td class="item-name">
						  <div class="delete-wpr">
							<button class="delete" on:click={() => removeRow(i)} title="Remove row">X</button>
						</div>
	
					
							<!-- svelte-ignore a11y-no-redundant-roles -->
							<textarea type="text" class="task"   on:input={resizeInput} bind:value={row.name} placeholder="Item"></textarea>
						</td>
							<td class="description">
							<textarea type="text" id="details" bind:value={row.description}  on:input={resizeInput} placeholder="Details"></textarea>
						</td>
						
				
						<td class="qty"><input type="text" bind:value={row.qty} class="qty" placeholder="0" on:change={() => updatePrice(row)}/></td>
						<td class="cost"><input type="text" bind:value={row.cost} class="cost" placeholder="$0" on:change={() => updatePrice(row)}/></td>
						
						<td><span class="price">${row.price}</span></td>
					
					</tr>
					
				
						<td colspan="5" class="addrow">
							
						  {#if i === rows.length - 1}
							<button type="button" id="addrow" on:click={addRow}>Add Row</button>
						  {/if}
						
						</td>
						
						{/each}
				
				  </tbody>
				
				  <tr>
					<td height="48" colspan="2" class="blank"></td>
					<td colspan="2" class="total-line">Subtotal</td>
					<td class="total-value"><div id="subtotal">${yourTotal.toFixed(2)}</div></td>
				  </tr>
				  <tr>
					<td colspan="2" class="blank"></td>
					<td colspan="2" class="total-line">Total</td>
					<td class="total-value"><div id="total">${yourTotal.toFixed(2)}</div></td>
				  </tr>
				  <tr>
					<td colspan="2" class="blank"></td>
					<td colspan="2" class="total-line">Amount Paid</td>
					<td width="86" class="total-value"><input type="text" id="paid" placeholder="$0.00"/></td>
				  </tr>
				  <tr>
					<td colspan="2" class="blank">&nbsp;</td>
					<td colspan="2" class="total-line balance">Balance Due</td>
					<td class="total-value balance"><div class="due">${yourAmountDue.toFixed(2)}</div></td>
				  </tr>
		
		  
		  </table>
		   
		  <div class="sigPad" id="smoothed-variableStrokeWidth" style="width:424px;">
  <h3>Signature</h3>
			  <input type="text" id="customer-name"  placeholder="Customer Name" />
  <br>    
		 
  
  <div class="sig sigWrapper" style="height:auto;">
  <div class="typed"></div>
  <canvas class="pad" width="420" height="100"></canvas>
  <input type="hidden" name="output-3" class="output">
  </div>
  
  <ul class="sigNav">
  <li class="clearButton"><a href="#clear">Clear</a></li>
  </ul>   
		  </div>	   	  
	  <div id="terms">
		<h5>Notes</h5>
		<textarea oninput="auto_grow(this)" rows="4" id="notes" textarea="textarea"></textarea>
		</div>
	  
  </div>
  </form>
  
  
  
  <div class="row">
	  <div class="col-sm-offset-5 col-sm-2 text-center">
  <button>Create PDF</button>
  </div>
  </div>
</main>