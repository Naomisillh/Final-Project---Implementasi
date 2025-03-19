from abc import ABC, abstractmethod
from datetime import datetime

class Payment(ABC):
    def __init__(self, payment_id, amount, date, status):
        self.payment_id = payment_id
        self.amount = amount
        self.date = date
        self.status = status
    
    @abstractmethod
    def processPayment(self):
        pass
    
    @abstractmethod
    def validatePayment(self):
        pass
    
    @abstractmethod
    def refundPayment(self):
        pass

class CreditCardPayment(Payment):
    def __init__(self, payment_id, amount, date, status, cardNumber, expiryDate, cvv):
        super().__init__(payment_id, amount, date, status)
        self.cardNumber = cardNumber
        self.expiryDate = expiryDate
        self.cvv = cvv
    
    def processPayment(self):
        if self.validatePayment():
            self.status = "Success"
            return f"Credit Card Payment of {self.amount} processed successfully."
        self.status = "Failed"
        return "Credit Card Payment failed."
    
    def validatePayment(self):
        return len(str(self.cardNumber)) in [15, 16] and len(str(self.cvv)) == 3
    
    def refundPayment(self):
        self.status = "Refunded"
        return f"Refund of {self.amount} to Credit Card successful."

class BankTransfer(Payment):
    def __init__(self, payment_id, amount, date, status, accountNumber, bankName, transferCode):
        super().__init__(payment_id, amount, date, status)
        self.accountNumber = accountNumber
        self.bankName = bankName
        self.transferCode = transferCode
    
    def processPayment(self):
        if self.validatePayment():
            self.status = "Success"
            return f"Bank Transfer of {self.amount} to {self.bankName} processed successfully."
        self.status = "Failed"
        return "Bank Transfer Payment failed."
    
    def validatePayment(self):
        return len(str(self.accountNumber)) >= 10 and len(str(self.transferCode)) == 6
    
    def refundPayment(self):
        self.status = "Refunded"
        return f"Refund of {self.amount} via Bank Transfer successful."

class DigitalWallet(Payment):
    def __init__(self, payment_id, amount, date, status, walletId, provider, phoneNumber):
        super().__init__(payment_id, amount, date, status)
        self.walletId = walletId
        self.provider = provider
        self.phoneNumber = phoneNumber
    
    def processPayment(self):
        if self.validatePayment():
            self.status = "Success"
            return f"Digital Wallet Payment of {self.amount} via {self.provider} processed successfully."
        self.status = "Failed"
        return "Digital Wallet Payment failed."
    
    def validatePayment(self):
        return len(str(self.phoneNumber)) == 10 and self.walletId is not None
    
    def refundPayment(self):
        self.status = "Refunded"
        return f"Refund of {self.amount} to Digital Wallet successful."

# Contoh Penggunaan
if __name__ == "__main__":
    cc_payment = CreditCardPayment("001", 100.0, datetime.now(), "Pending", "1234567812345678", "12/26", "123")
    print(cc_payment.processPayment())
    print(cc_payment.refundPayment())
    
    bank_payment = BankTransfer("002", 200.0, datetime.now(), "Pending", "9876543210", "Bank XYZ", "123456")
    print(bank_payment.processPayment())
    print(bank_payment.refundPayment())
    
    wallet_payment = DigitalWallet("003", 50.0, datetime.now(), "Pending", "WAL12345", "PayPal", "0812345678")
    print(wallet_payment.processPayment())
    print(wallet_payment.refundPayment())
