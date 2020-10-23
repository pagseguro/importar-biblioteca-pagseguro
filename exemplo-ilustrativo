<?php

	$url = "https://ws.sandbox.pagseguro.uol.com.br/v2/sessions";

	$credenciais = array(
	    	"email" => "xxx",
			"token" => "xxx"
	);
	$curl = curl_init($url);
	curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, false);
	curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($curl, CURLOPT_POSTFIELDS, http_build_query($credenciais));
	$resultado = curl_exec($curl);
	curl_close($curl);
	$session = simplexml_load_string($resultado)->id;
?>
<!DOCTYPE html>
<html>
<meta charset="utf-8">
<head>
	<title>Checkout Transparente PagSeguro</title>
	<script type="text/javascript"></script>
	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
</head>
<body>
	<div class="text-center">
	<img src="https://assets.pagseguro.com.br/ps-website-assets/v13.13.22/img/for-your-business/online/checkout/banner-image.png" class="img-fluid" alt="Responsive image">
	
	<h1 class = "text-center">CHECKOUT TRANSPARENTE DEMO - PAGSEGURO [SANDBOX]</h1><hr>
	</div>
	<fieldset>
	<div class="row mx-md-n5">
  	<div class="col px-md-5"><div class="p-3 border bg-light">
	  
	<legend class="text-center">GERAR SENDERHASH</legend>
	<div> 
	<input class="form-control" type="text" id="senderHash" class="creditcard" name="senderHash">
	<button class="btn btn-info" id="generateSenderHash">Gerar</button>
	
	</div>	</div>	</div>

	</fieldset>
	<br>

	<fieldset>
	<legend class="text-center">CHAMADAS PARA CARTÃO DE CRÉDITO</legend>


	<div class="row mx-md-n5">
  	<div class="col px-md-5"><div class="p-3 border bg-light">
	<div class="row">
		<div class="col-sm-4"> 
		<div>
		<label for="creditCardNumber"<b>Número do cartão:</b></label> 
		<input type="text" class="form-control" id="creditCardNumber" class="creditcard" name="creditCardNumber">
		</div>
		<div>
		<label for="creditCardBrand"<b>Bandeira:</b></label>
		<input type="text" class="form-control" id="creditCardBrand" class="creditcard" name="creditCardBrand" disabled>
		</div></div>
		<div class="col-sm-4">
		<label for="creditCardExpMonth"<b>Validade Mês (mm):</b></label>
		<input type="text" class="form-control" id="creditCardExpMonth" class="creditcard" name="creditCardExpMonth" size="2"> &nbsp;

		 
		<label for="creditCardExpYear"<b>Ano (yyyy):</b></label>
		<input type="text" class="form-control" id="creditCardExpYear" class="creditcard" name="creditCardExpYear" size="4">
		</div>

		<div class="col-sm-4">
		<label for="creditCardCvv"<b>CVV:</b></label>
		 <input type="text" class="form-control" id="creditCardCvv" class="creditcard" name="creditCardCvv">
	
		 <label <b>Token:</b></label>
		 <input type="text" id="creditCardToken" class="form-control" name="creditCardToken" disabled>
	 	 <button  class="btn btn-info" id="generateCreditCardToken">Gerar Token</button>
	
	
	</div>
	</div>
	</div></div>

	</fieldset>
	<br>
	<fieldset>
	<legend class="text-center">PARCELAMENTO</legend>
	<div class="row mx-md-n5">
  	<div class="col px-md-5"><div class="p-3 border bg-light">
		Valor do Checkout: <input class="form-control" type="text" id="checkoutValue" name="checkoutValue">
	<button class="btn btn-info" id="installmentCheck">Ver Parcelamento</button>
	</p>
	<p>
	<select id="InstallmentCombo">
	</select>
	</p>
	</div>
	</div>
	</fieldset>
	
</body>
<!-- Incluíndo o arquivo JS do PagSeguro e o JQuery-->
<script type="text/javascript" src="https://stc.sandbox.pagseguro.uol.com.br/pagseguro/api/v2/checkout/pagseguro.directpayment.js"></script>
"></script>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<!-- Funcionalidade do JS -->
<script type="text/javascript">

	//Session ID
	PagSeguroDirectPayment.setSessionId('<?php echo $session; ?>');
	console.log('<?php echo $session; ?>');

	//Get SenderHash
	$("#generateSenderHash").click(function(){
		PagSeguroDirectPayment.onSenderHashReady(function(response){
    if(response.status == 'error') {
        console.log(response.message);
        return false;
    }
	//Hash estará disponível nesta variável.
	$("#senderHash").val(response.senderHash);	
});
		
	})

	//Get CreditCard Brand and check if is Internationl
	$("#creditCardNumber").keyup(function(){
		if ($("#creditCardNumber").val().length >= 6) {
			PagSeguroDirectPayment.getBrand({
				cardBin: $("#creditCardNumber").val().substring(0,6),
				success: function(response) { 
						console.log(response);
						$("#creditCardBrand").val(response['brand']['name']);
						$("#creditCardCvv").attr('size', response['brand']['cvvSize']);
				},
				error: function(response) {
					console.log(response);
				}
			})
		};
	})

	function printError(error){
		$.each(error['errors'], (function(key, value){
			console.log("Foi retornado o código " + key + " com a mensagem: " + value);
		}));
	}

	function getPaymentMethods(valor){
		PagSeguroDirectPayment.getPaymentMethods({
			amount: valor,
			success: function(response) {
				//console.log(JSON.stringify(response));
				console.log(response);
			},
			error: function(response) {
				console.log(JSON.stringify(response));
			}
		})
	}	

	//Generates the creditCardToken
	$("#generateCreditCardToken").click(function(){
		var param = {
			cardNumber: $("#creditCardNumber").val(),
			cvv: $("#creditCardCvv").val(),
			expirationMonth: $("#creditCardExpMonth").val(),
			expirationYear: $("#creditCardExpYear").val(),
			success: function(response) {
				console.log(response);
				$("#creditCardToken").val(response['card']['token']);
			},
			error: function(response) {
				console.log(response);
				printError(response);
			}
		}
			//parâmetro opcional para qualquer chamada
			if($("#creditCardBrand").val() != '') {
				param.brand = $("#creditCardBrand").val();
			}
			PagSeguroDirectPayment.createCardToken(param);
	})

	//Check installment
	$("#installmentCheck").click(function(){
		if($("#creditCardBrand").val() != '') {
			getInstallments();
		} else {
			alert("Uma bandeira deve estar selecionada");
		}
	})

	function getInstallments(){
		var brand = $("#creditCardBrand").val();
		PagSeguroDirectPayment.getInstallments({
			amount: $("#checkoutValue").val().replace(",", "."),
			brand: brand,
			maxInstallmentNoInterest: 2, //calculo de parcelas sem juros
			success: function(response) {
				var installments = response['installments'][brand];
				buildInstallmentSelect(installments);
			},
			error: function(response) {
				console.log(response);
			}
		})
	}

	function buildInstallmentSelect(installments){
		$.each(installments, (function(key, value){
			$("#InstallmentCombo").append("<option value = "+ value['quantity'] +">" + value['quantity'] + "x de " + value['installmentAmount'].toFixed(2) + " - Total de " + value['totalAmount'].toFixed(2)+"</option>");
		}))
	}
</script>

</html>
