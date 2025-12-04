# Simple-Cashiering-System-
Simple Cashiering System 
print("===== WELCOME TO SIMPLE CASHIERING SYSTEM =====")

daily_summary = {
    'total_sales': 0.0,
    'cash_payments': 0.0
}

username = input("Enter username: ")
password = input("Enter password: ")

if username == "admin" and password == "1234":
    print("\nLogin successful! Welcome,", username)

    total = 0.0
    cart = []
    
    stock = {
        1: {'name': 'Mang Juan', 'price': 15.00, 'available': 10},
        2: {'name': 'Clover', 'price': 12.00, 'available': 5},
        3: {'name': 'Piattos', 'price': 20.00, 'available': 8}
    }

    while True:
        print("\n===== MENU =====")
        for key, item in stock.items():
            print(f"{key}. {item['name']} (₱{item['price']:.2f} | {item['available']} pcs available)")
        print("4 Logout") 
        print("5. Exit Program")

        try:
            choice = int(input("\nEnter choice number: "))

            if choice == 4:
                print("\nLogout...")
                break 

            elif choice == 5:
                print("\nExiting Program..")
                break

            elif choice in stock:
                item_data = stock[choice]
                item_name = item_data['name']
                price = item_data['price']
                available_stock = item_data['available']

                pcs = int(input(f"How many pcs of {item_name} (Max: {available_stock}): "))

                if pcs <= 0 or pcs > available_stock:
                    print(f"\nInvalid quantity or insufficient stock! Available: {available_stock} pcs.")
                    continue

                item_total = price * pcs
                total += item_total
                stock[choice]['available'] -= pcs

                cart.append({
                    'item': item_name,
                    'price': price,
                    'pcs': pcs,
                    'item_total': item_total
                })

                again = input("Do you want to buy again? (yes/no): ").lower()
                if again != "yes":
                    break

            else:
                print("\nInvalid choice! Please try again.")

        except ValueError:
            print("\nInvalid input! Please enter a number.")

    if not cart:
        print("\nNo items purchased.")
    else:
        print(f"\nTotal to pay: ₱{total:.2f}")
        while True:
            try:
                amount_paid = float(input("Enter amount payment: ₱"))
                if amount_paid >= total:
                    break
                else:
                    print("\nInsufficient payment! Try again.")
            except ValueError:
                print("\nInvalid input! Enter a valid amount.")

        change = amount_paid - total
        daily_summary['cash_payments'] += amount_paid
        daily_summary['total_sales'] += total

        print("\n===== RECEIPT =====")
        for item in cart:
            print(f"Item: {item['item']} | Pcs: {item['pcs']} | Price: ₱{item['price']:.2f} | Total: ₱{item['item_total']:.2f}")
        print("---------------------------")
        print(f"TOTAL: ₱{total:.2f}")
        print(f"AMOUNT PAID: ₱{amount_paid:.2f}")
        print(f"CHANGE: ₱{change:.2f}")
        print("===== END OF RECEIPT =====")

    print("\n--- DAILY SUMMARY ---")
    print(f"Total Sales: ₱{daily_summary['total_sales']:.2f}")
    print(f"Total Cash Payments Recorded: ₱{daily_summary['cash_payments']:.2f}")
    print("---------------------")
    print("\nThank you for visiting! Program ended.")

else:
    print("\nInvalid login! Exiting program.")
