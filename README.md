# django-testing-guide

An unofficial one page guide of testing Django


* Bad Smells
    * More that one responsability
    * Put the code in the right place
    * A lot of mocks
    * Don't test the framework, test your application.
    * Don't test bugs.

* Testing Models
    * Testing custom method of my model
    * Testing a validator

* Testing Forms
    * Test Validation
    * Testing my custom field

* Testing Views
    * Using Request Factory
    * Testing a GET request
    * Testing a POST request
    * How to test a context
    * How to test a redirect
    * How to test a Mixin

* Testing Template Tags

* Fixtures

* Mocks and Patchs

## Models

### Method Override

``` python
# models.py
import datetime

class Product(models.Product):
     name = models.CharField(max_length=100)
     price = models.DecimalField(max_digits=10, decimal_fields=2)

class Order(models.Model):
    product = models.ForeignKey(Product)
    amount = models.DecimalField(max_digits=10, decimal_fields=2)
    created_at = models.DateTimeField()

    def subtotal(self):
        return self.product.price * self.amount

# tests.py
import datetime
from decimal import Decimal

from django.tests import TestCase

from .models import Product, Order

class OrderTestCase(TestCase):

    def setUp(self):
        product = Product(price=Decimal('10.5'))

        self.order = Order(
             product = product,
             amount = 10,
             created_at = datetime.datetime.now())

    def test_subtotal(self):
        self.assertEqual(Decimal('105'), self.order.subtotal())
```
