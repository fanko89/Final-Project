<script>
	export let date;
	import './lib/styles/styles.css';


</script>

<main><form class="ui form">



			
	<div id="page-wrap">
  
		  <textarea class="header">INVOICE</textarea>
		  
		  <div id="info">
			<p><strong>155, UT 84443 | (801) 123 1234 | Fax: (801) 123 1234| website.com | office@website.com</strong></p></div>    
		  
			<div class="logo">
				<img id="image" src="/img/logo.png" alt="logo" />
			  </div>
		  
	   
		  
	
		  <div id="identity">
		  
		  <h3>Customers Info:</h3>
					  
		<input type="text" textarea id="name" placeholder="Customer Name"  />
		<input type="text" textarea id="address" placeholder="Street Address" />
		<input type="text" textarea id="region" placeholder="City, State Zip" />
		<input type="text" textarea id="phone" placeholder="Phone Number" />
		<input type="text" textarea id="email" placeholder="Email Address" />
	</div> 
	      
  
	<div style="clear:both"></div>
		  
	<div id="tec">
  <h3>Technician Name:</h3>
  <br />
		  <input type="text" textarea id="tecName"  placeholder="Your Name" />
  
			  <table id="data">
				  <tr>
					  <td class="invoiceNum">Invoice #</td>
					  <td> <textarea id="randomNumber"></textarea></td>
				  </tr>
				  <tr>
					  <td class="theDate">Date</td>
					  <td><input type="text" textarea id="date"  placeholder="{date}" /></td>
				  </tr>
				  <tr>
					  <td class="amountDue">Amount Due</td>
					  <td><div class="due">$0.00</div></td>
				  </tr>
			  </table>
		</div>
	  <table width="100%" id="items"> 
			<tr>
				<th width="50">Employee</th>
				<th width="219">Description/Material</th>
				<th width="46">Cost</th>
				<th width="46">Hours</th>
				<th width="46">Price</th>
			</tr>
			
		   <tr class="item-row">
				<td class="item-name"><div class="delete-wpr">
				  <textarea oninput="auto_grow(this)" rows="3" id="task" textarea="textarea" placeholder="item/task completed"></textarea>
			  <!-- <a class="delete" href="javascript:;" title="Remove row">X</a></div></td> -->
				<td class="description"><textarea oninput="auto_grow(this)" rows="3" id="details" textarea="textarea" placeholder="Details"></textarea></td>
				<td><input type="text" width="70px" textarea class="cost"  placeholder="$0"  /></td>
				<td><input type="text" width="70px" textarea class="qty" placeholder="0" /></td>
				<td><span class="price">$0</span></td>
			</tr>
			
			<tr id="hiderow">
			  <!-- <td colspan="5"><a id="addrow" href="javascript:;" title="Add a row"> add a row</a></td> -->
			</tr>
			
			<tr>
				<td height="48" colspan="2" class="blank"></td>
			  <td colspan="2" class="total-line">Subtotal</td>
				<td class="total-value"><div id="subtotal">$0.00</div></td>
			</tr>
			<tr>
  
				<td colspan="2" class="blank"></td>
				<td colspan="2" class="total-line">Total</td>
				<td class="total-value"><div id="total">$0.00</div></td>
			</tr>
			<tr>
				<td colspan="2" class="blank"></td>
				<td colspan="2" class="total-line">Amount Paid</td>
  
				<td width="86" class="total-value"><input type="text" textarea id="paid"  placeholder="$0.00" /></td>
			</tr>
			<tr>
				<td colspan="2" class="blank">&nbsp;</td>
				<td colspan="2" class="total-line balance">Balance Due</td>
				<td class="total-value balance"><div class="due">$0.00</div></td>
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
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
  <br>
		
		  
		  
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
  <br/>
  <br/>
  <br/>
	<h1>Hello {name}!</h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>