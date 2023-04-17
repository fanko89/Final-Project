<script>
  import { each } from 'svelte/internal';
	import './lib/styles/styles.css';

 
	let yourData = [];
  let yourTotal = 0;
  let yourAmountDue = 0;

  function getCurrentDate() {
    const today = new Date();
    const dd = String(today.getDate()).padStart(2, '0');
    const mm = String(today.getMonth() + 1).padStart(2, '0');
    const yyyy = today.getFullYear();
    return mm + '/' + dd + '/' + yyyy;
  }

  function deleteRow(rowId) {
    yourData.splice(rowId, 1);
    updateTotalAndAmountDue();
  }

  function addRow() {
    const newRow = {
      description: '',
      amount: 0,
      paid: 0
    };
    yourData.push(newRow);
    updateTotalAndAmountDue();
  }

  function updatePrice(rowId, cost, qty) {
    const row = yourData[rowId];
    row.amount = cost * qty;
    row.price = `$${row.amount.toFixed(2)}`;
    updateTotalAndAmountDue();
  }

  function updateTotalAndAmountDue() {
    let total = 0;
    let amountDue = 0;
    yourData.forEach((row) => {
      total += row.amount;
      amountDue += row.amount - row.paid;
    });
    yourTotal = total;
    yourAmountDue = amountDue;

    document.querySelector('#subtotal').textContent = `$${yourTotal.toFixed(2)}`;
    document.querySelector('#total').textContent = `$${yourTotal.toFixed(2)}`;
    document.querySelector('.due').textContent = `$${yourAmountDue.toFixed(2)}`;
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
		  <input type="text" textarea id="name" placeholder="Customer Name" />
		  <input type="text" textarea id="address" placeholder="Street Address" />
		  <input type="text" textarea id="region" placeholder="City, State Zip" />
		  <input type="text" textarea id="phone" placeholder="Phone Number" />
		  <input type="text" textarea id="email" placeholder="Email Address" />
		</div>
		<div style="clear:both"></div>
		<div id="customer">
		  <h3>Technician Name:</h3>
		  <br />
		  <input type="text" textarea id="customer-title" placeholder="Your Name" />
		  <table id="meta">
			<tr>
			  <td class="meta-head">Invoice #</td>
			  <td> <textarea id="randomNumber"></textarea></td>
			</tr>
				  <tr>
					  <td class="meta-head">Date</td>
					  <td><input type="text" textarea id="date"  placeholder={getCurrentDate()} /></td>
				  </tr>
				  <tr>
					<td class="meta-head">Amount Due</td>
					<td><div class="due">${yourAmountDue.toFixed(2)}</div></td>
				  </tr>
				</table>
			  </div>
			  <table width="100%" id="items">
				<thead>
				  <tr>
					<th width="50">Employee</th>
					<th width="219">Description/Material</th>
					<th width="46">Cost</th>
					<th width="46">Quant</th>
					<th width="46">Price</th>
				  </tr>
				</thead>
				<tbody>
				  {#each yourData as row, i}
					<tr class="item-row">
					  <td class="item-name">
						<div class="delete-wpr">
						  <textarea oninput="auto_grow(this)" rows="3" id="task" textarea="textarea" placeholder="item/task completed"></textarea>
						  <button class="delete" on:click={() => deleteRow(i)} title="Remove row">X</button>
						</div>
					  </td>
					  <td class="description">
						<textarea oninput="auto_grow(this)" rows="3" id="details" textarea="textarea" placeholder="Details"></textarea>
					  </td>
					  <td>
						<input type="text" width="70px" textarea class="cost" placeholder="$0" on:input={(e) => updatePrice(i, e.target.value, row.qty)} bind:value={row.cost} />
					  </td>
					  <td>
						<input type="text" width="70px" textarea class="qty" placeholder="0" on:input={(e) => updatePrice(i, row.cost, e.target.value)} bind:value={row.qty} />
					  </td>
					  <td>{row.price}</td>
					</tr>
				  {/each}
				  <tr id="hiderow">
					<td colspan="5"><a id="addrow" on:click={() => addRow()} title="Add a row">add a row</a></td>
				  </tr>
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
					<td width="86" class="total-value"><input type="text" textarea id="paid" placeholder="$0.00" /></td>
				  </tr>
				  <tr>
					<td colspan="2" class="blank">&nbsp;</td>
					<td colspan="2" class="total-line balance">Balance Due</td>
					<td class="total-value balance"><div class="due">${yourAmountDue.toFixed(2)}</div></td>
				  </tr>
		
		  
		  </table>	  
		  <div class="sigPad" id="smoothed-variableStrokeWidth" style="width:424px;">
  <h3>Signature</h3>
			  <input type="text" textarea id="customer-title"  placeholder="Customer Name" />
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
  <div class="btn btn-danger btn-lg" id="create_pdf">Create PDF
  </div>
  </div>
  </div>
</main>