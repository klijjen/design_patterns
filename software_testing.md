# Лаба 6. Тестирование ПО


```Java
public class BankAccount {
    private String accountNumber;
    private double balance;

    public BankAccount (String accountNumber, double balance) {
        if (balance < 0) {
            throw new IllegalArgumentException("Initial balance cannot be negative");
        }

        this.accountNumber = accountNumber;
        this.balance = balance;
    }

    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Deposit amount must be positive");
        }

        this.balance += amount;
    }

    public void withdraw(double amount)
    {
        if (amount <= 0) {
            throw new IllegalArgumentException("Withdrawal amount must be positive");
        }
        if (this.balance < amount) {
            throw new IllegalArgumentException("Insufficient funds");
        }

        this.balance -= amount;
    }

    public double getBalance() {
        return this.balance;
    }
}

```


```Java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

import org.mockito.Mockito;

public class BankAccountTest {
    private BankAccount account;

    @BeforeEach
    public void setUp() {
        account = new BankAccount("123456", 100.0);
    }

    @Test
    public void testInitialBalanceValid() {
        BankAccount newAccount = new BankAccount("654321", 50.0);
        assertEquals(50.0, newAccount.getBalance());
    }

    @Test
    public void testInitialBalanceInvalid() {
        assertThrows(IllegalArgumentException.class, () -> new BankAccount("654321", -10.0));
    }

    @Test
    public void testDepositSuccess() {
        account.deposit(50.0);
        assertEquals(150.0, account.getBalance());
    }

    @Test
    public void testDepositInvalidAmount() {
        assertThrows(IllegalArgumentException.class, () -> account.deposit(-20.0));
        assertThrows(IllegalArgumentException.class, () -> account.deposit(0.0));
    }

    @Test
    public void testWithdrawSuccess() {
        account.withdraw(40.0);
        assertEquals(60.0, account.getBalance());
    }

    @Test
    public void testWithdrawInsufficientFunds() {
        Exception exception = assertThrows(IllegalArgumentException.class, () -> account.withdraw(200.0));
        assertEquals("Insufficient funds", exception.getMessage());
    }

    @Test
    public void testWithdrawInvalidAmount() {
        assertThrows(IllegalArgumentException.class, () -> account.withdraw(-10.0));
        assertThrows(IllegalArgumentException.class, () -> account.withdraw(0.0));
    }

    @Test
    public void testBalanceAfterOperations() {
        account.deposit(50.0);
        account.withdraw(30.0);
        assertEquals(120.0, account.getBalance());
    }
}
```
