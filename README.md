import bisect
import pandas as pd
class Orderbook(object):
  def_init_(self):
     self.order_history = []
     self._bid_book = {}
     self._bid_book_prices = []
     self._ask_book = {}
     self._ask_book_prices []
     self.confirm_modify_collector = []
     self.confirm_trade_collector = []
     self._sip_collector = []
     self._trade_book = []
     self._order_index = 0
     self.traded = False
 def _add_order_to_history(self, order):
    '''Add an order (dict) to order_history'''
    hist_order = {'order_id': order['order_id'], 'timestamp': order['timestamp'], 'type': order['type'],
                  'quantity': order['quantity'], 'side': order['side'], 'price': order['price']}
    self._order_index += 1
    hist_order['exid'] = self._order_index
    self.order_history.append(hist_order)
def add_order_to_book(self, order):
    '''
    Use insort to maintain on ordered list of prices which serve as pointers
    to the orders.
    '''
    book_order = {'order_id': order['order_id'], 'timestamp': order['timestamp'], 'type': order['type'],
                  'quantity': order['quantity'], 'side': order['side'], 'price': order['price']}
    if order['side'] == 'buy':
        book_prices = self._bid_book_prices
        book = self._bid_book
    else:
        book_prices = self._ask_book_prices
        book = self._ask_book
    if order['price'] in book_prices:
        book[order['price']]['num_orders'] += 1
        book[order['price']]['size'] += order['quantity']
        book[order['price']]['order_ids'].append(order['order_id'])
        book[order['price']]['orders'][order['order_id']] = book_order
    else:
        bisect.insort(book_prices, order['price'])
        book[order['price']] = {'num_orders': 1, 'size': order['quantity'], 'order_ids': [order['order_id']],
                                'orders': {order['order_id']: book_order}}
def _remove_order(self, order_side, order_price, order_id):
    '''Pop the order_id; if  order_id exists, updates the book.'''
    if order_side == 'buy':
        book_prices = self._bid_book_prices
        book = self._bid_book
    else:
        book_prices = self._ask_book_prices
        book = self._ask_book
    is_order = book[order_price]['orders'].pop(order_id, None)
    if is_order:
        book[order_price]['num_orders'] -= 1
        book[order_price]['size'] -= is_order['quantity']
        book[order_price]['order_ids'].remove(is_order['order_id'])
        if book[order_price]['num_orders'] == 0:
            book_prices.remove(order_price)
