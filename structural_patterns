# Лаба 4. Структурные паттерны проектирования

##Proxy (прокси) 
```Java
public class Main {
    interface BankServer {
        String getAccountDetails(String accountNumber);
    }

    // Реальный сервер
    public static class RealBankServer implements BankServer {
        @Override
        public String getAccountDetails(String accountNumber) {
            return "Account details for " + accountNumber;
        }
    }

    public static class BankServerProxy implements BankServer {
        private RealBankServer realServer;
        private boolean hasAccess;

        public BankServerProxy(boolean hasAccess) {
            this.realServer = new RealBankServer();
            this.hasAccess = hasAccess;
        }

        @Override
        public String getAccountDetails(String accountNumber) {
            if (!hasAccess) {
                return "Access denied. Unable to retrieve account details.";
            }
            String details = realServer.getAccountDetails(accountNumber);
            return details;
        }
    }

    public static void main(String[] args) {
        BankServer userProxy = new BankServerProxy(false);
        BankServer adminProxy = new BankServerProxy(true);

        System.out.println(userProxy.getAccountDetails("12345"));
        // Вывод: Access denied. Unable to retrieve account details.

        System.out.println(adminProxy.getAccountDetails("12345"));
        // Вывод: Account details for 12345
    }
}
```


##Adapter (адаптер)
```Java
public class Main {
    public interface Time24Hour {
        String getTime24Hour();
        void setTime24Hour(String time);
    }

    public static class Time12Hour {
        private String time;

        public String getTime12Hour() {
            return time;
        }

        public void setTime12Hour(String time) {
            this.time = time;
        }
    }

    // Адаптер для преобразования между форматами
    public static class TimeAdapter implements Time24Hour {
        private Time12Hour time12Hour;

        public TimeAdapter(Time12Hour time12Hour) {
            this.time12Hour = time12Hour;
        }

        @Override
        public String getTime24Hour() {
            String time = time12Hour.getTime12Hour();
            return convertTo24Hour(time);
        }

        @Override
        public void setTime24Hour(String time) {
            String convertedTime = convertTo12Hour(time);
            time12Hour.setTime12Hour(convertedTime);
        }

        // Преобразование 12-часового формата в 24-часовой
        private String convertTo24Hour(String time12) {
            String[] parts = time12.split(" ");
            String[] timeParts = parts[0].split(":");
            int hours = Integer.parseInt(timeParts[0]);
            String minutes = timeParts[1];

            if (parts[1] == "PM" && hours != 12) {
                hours += 12;
            }
            else if (parts[1] == "AM" && hours == 12) {
                hours = 0;
            }
            return String.format("%02d:%s", hours, minutes);
        }

        private String convertTo12Hour(String time24) {
            String[] timeParts = time24.split(":");
            int hours = Integer.parseInt(timeParts[0]);
            String minutes = timeParts[1];
            String period = "AM";

            if (hours >= 12) {
                period = "PM";
                if (hours > 12) {
                    hours -= 12;
                }
            } else if (hours == 0) {
                hours = 12;
            }

            return String.format("%02d:%s %s", hours, minutes, period);
        }
    }

    public static void main(String[] args) {
        Time12Hour time12 = new Time12Hour();
        
        time12.setTime12Hour("02:30 PM");
        Time24Hour adapter = new TimeAdapter(time12);
        System.out.println("Time in 24-hour format: " + adapter.getTime24Hour());
        // Вывод: Time in 24-hour format: 14:30

        adapter.setTime24Hour("23:15");
        System.out.println("Time in 12-hour format: " + time12.getTime12Hour());
        // Вывод: Time in 12-hour format: 11:15 PM
    }
}
```


##Bridge (мост)
```Java
public class Main {
    static abstract class UserImpl {
        public abstract void run();
        public abstract void fly();
    }

    static class UserDonkeyImpl extends UserImpl {
        @Override
        public void run() {
            System.out.println("Donkey is running...");
        }

        @Override
        public void fly() {
            System.out.println("Donkey can't fly!");
        }
    }

    static class UserDragonImpl extends UserImpl {
        @Override
        public void run() {
            System.out.println("Dragon is running...");
        }

        @Override
        public void fly() {
            System.out.println("Dragon is flying!");
        }
    }

    static class User {
        private UserImpl realUser;

        public User(UserImpl impl) {
            realUser = impl;
        }

        public void run() {
            realUser.run();
        }

        public void fly() {
            realUser.fly();
        }
        public void transformToDonkey() {
            realUser = new UserDonkeyImpl();
        }

        public void transformToDragon() {
            realUser = new UserDragonImpl();
        }
    }

    public static void main(String[] args) {
        User user = new User(new UserDonkeyImpl());

        user.run(); // Вывод: Donkey is running...
        user.fly(); // Вывод: Donkey can't fly!

        user.transformToDragon();
        user.run(); // Вывод: Dragon is running...
        user.fly(); // Вывод: Dragon is flying!
    }
}
```
