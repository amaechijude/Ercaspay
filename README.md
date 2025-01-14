# ErcasPay
A python package that simplifies https://docs.ercaspay.com/ payment api integration into your python projects.

## Installation
This package requires the request library to be installed
```bash
pip install requests==2.32.3
pip install python-ercas-ng
```

## Usage
```python
from Ercasng.ercaspay import ErcasPay

## Do not hardcode your secret key inside your code

## Instanstiate ErcasPay Object with your ercasPay secret key

ercas_object = ErcasPay(ercas_pay_secret_key: str)

```

### Initiate Transaction Demo
```python
#This function/method could be a view for django, flask, fastapi or something else.

def initialize_transaction(request):
    amount = 100.00 # Amount for the transaction. Mandatory
    payment_refrence = "R5md7gd9b4s3h2j5d67g" # Payment refrence for the transaction. Mandatory
    customer_name = "FirstName LastName" # Customer name. Mandatory
    customer_email = "customer@example.com" # Customer email. Mandatory
    customer_phone = "08012345678" # Customer phone number. Optional
    redirect_url = "http://redirect.example.com" #Redirect URL. Optional
    description = "Description" # Description. Optional

    ## if the target customer will pay in naira:
        # use the ngn_init method
    # else 
        # use the usd_init method.

    ## Initiate Naira Transaction
    ngn_init = ercas_object.naira_initiate_transaction(self, amount:float, payment_reference:str,
                                   customer_name:str, customer_email:str,
                                   customer_phone:str = None, redirect_url:str=None,
                                   descrption:str = None)

    ## Initiate USD Transaction
    usd_init = ercas_object.usd_initiate_transaction(self, amount:float, payment_reference:str,
                                   customer_name:str, customer_email:str,
                                   customer_phone:str = None, redirect_url:str=None,
                                   descrption:str = None)


     ## handle Successful response for any of the above methods
     if ngn_init["tx_status"]:
        return {
            "tx_status": True,
            "tx_code": "success",
            "tx_body": {
                "payment_reference": "R5md7gd9b4s3h2j5d67g",
                "tx_reference": "ERCS|20231113082706|1699860426792",
                "checkoutUrl": "https://sandbox-checkout.ercaspay.com/ERCS|20231113082706|1699860426792"
            }
        }

        ## The Checkout URL is required and should be returned to and loaded from the frontend to complete the payment checkout

    ## Handle Unsuccessful Response
    return {
                "tx_status": False,
                "tx_code": "failed",
                "error_message": "Actual error that occurred",
                "tx_body": []
            }

```

### Verify Transaction Demo
```python
#This function/method could be a view for django, flask, fastapi or something else.

def verify_transaction(request):
    transaction_reference = "transaction_reference"
    verify_tx = ercas_object.verify_transaction(transaction_reference)

    ## Handle Successful Response
    if verify_tx["tx_status"]:
        return {
            "tx_status": True,
            "tx_code": "success",
            "tx_message": "Transaction fetched successfully",
            "tx_body": {
                "domain": "test",
                "status": "SUCCESSFUL",
                "ercs_reference": "ERCS|20231112152333|1699799013942",
                "tx_reference": "CSHM|WLTP|48606GWR",
                "amount": 100,
                "description": null,
                "paid_at": "2023-11-12T14:24:58.000000Z",
                "created_at": "2023-11-12T14:23:33.000000Z",
                "channel": "BANK_TRANSFER",
                "currency": "NGN",
                "metadata": "{\"firstname\":\"\",\"lastname\":\"Benson\",\"email\":\"iie@mail.com\"}",
                "fee": 1.4,
                "fee_bearer": "customer",
                "settled_amount": 100,
                "customer": {
                "name": "FirstName LastName",
                "phone_number": "09061628409",
                "email": "customer@email.com",
                "reference": "ZEKvFI-N8lMHY"
                }
            }

        ## Handle Unccessful Response
        if not verify_tx["tx_status"]:
            return {
                "tx_status": False,
                "error_message": "Actual error message",
                "tx_code": "failed",
                "tx_body": []
                }

```


