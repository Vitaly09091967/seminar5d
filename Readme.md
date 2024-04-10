Данный код описывает модели Django для клиентов, товаров и заказов, а также настройки

административной панели для удобного просмотра и управления данными.

models.py:

from django.db import models

class Client(models.Model):
    name = models.CharField(max_length=100)
    email = models.EmailField()

    def __str__(self):
        return self.name

class Product(models.Model):
    name = models.CharField(max_length=100)
    description = models.TextField()
    price = models.DecimalField(max_digits=10, decimal_places=2)

    def __str__(self):
        return self.name

class Order(models.Model):
    client = models.ForeignKey(Client, on_delete=models.CASCADE)
    products = models.ManyToManyField(Product)

    def __str__(self):
        return f"Order #{self.pk}"

admin.py:

from django.contrib import admin
from .models import Client, Product, Order

class ClientAdmin(admin.ModelAdmin):
    list_display = ('name', 'email')

class ProductAdmin(admin.ModelAdmin):
    list_display = ('name', 'price')

class OrderAdmin(admin.ModelAdmin):
    list_display = ('id', 'client', 'get_product_names')

    def get_product_names(self, obj):
        return ", ".join([product.name for product in obj.products.all()])

    get_product_names.short_description = 'Products'  # Определение заголовка для столбца

admin.site.register(Client, ClientAdmin)
admin.site.register(Product, ProductAdmin)
admin.site.register(Order, OrderAdmin)

Этот код определяет модели Client, Product и Order, а также настраивает их отображение в 

административной панели Django.

Для модели Client и Product указаны поля для отображения в списке объектов.

Для модели Order добавлен метод get_product_names, который возвращает строку с названиями 

продуктов, связанных с заказом, что делает удобным просмотр связанных продуктов прямо из списка 

заказов.

Зарегистрированы созданные классы администраторов для каждой модели с помощью admin.site.register.

Таким образом, административная панель будет отображать информацию о клиентах, товарах и заказах

 в удобном формате для просмотра и управления данными.