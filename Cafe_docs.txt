from tkinter import*
from tkinter import ttk
import random
from tkinter import messagebox 
import time
import qrcode
from PIL import Image, ImageTk
import os
x=random.randint(12980, 50876)
randomRef = str(x)
root = Tk()
root.geometry("890x580+0+0")
root.title("CAFE BILLING SYSTEM")

Tops = Frame(root,bg="white",width = 1600,height=50,relief=SUNKEN)
Tops.pack(side=TOP)

f1 = Frame(root,width = 900,height=700,relief=SUNKEN)
f1.pack(side=LEFT)

f2 = Frame(root ,width = 400,height=700,relief=SUNKEN)
f2.pack(side=RIGHT)
#------------------TIME--------------
localtime=time.asctime(time.localtime(time.time()))
#-----------------INFO TOP------------
lblinfo = Label(Tops, font=( 'aria' ,30, 'bold' ),text="CAFE BILLING MANAGEMENT SYSTEM",fg="Black",bd=10,anchor='w')
lblinfo.grid(row=0,column=0)
lblinfo = Label(Tops, font=( 'aria' ,20, ),text=localtime,fg="steel blue",anchor=W)
lblinfo.grid(row=1,column=0)



def GenerateBill0():
    # Create the bill content for the QR code
    bill_content = f"Order No: {rand.get()}\n"
    bill_content += f"French Fries: {Fries.get()}\n"
    bill_content += f"Lunch: {Largefries.get()}\n"
    bill_content += f"Burger: {Burger.get()}\n"
    bill_content += f"Pizza: {Filet.get()}\n"
    bill_content += f"Cheese Burger: {Cheese_burger.get()}\n"
    bill_content += f"Drinks: {Drinks.get()}\n"
    bill_content += f"Subtotal: {Subtotal.get()}\n"
    bill_content += f"Tax: {Tax.get()}\n"
    bill_content += f"Service Charge: {Service_Charge.get()}\n"
    bill_content += f"Total: {Total.get()}"

    # Create a QR code for the bill content
    qr = qrcode.QRCode(
        version=1,
        error_correction=qrcode.constants.ERROR_CORRECT_L,
        box_size=10,
        border=4,
    )
    qr.add_data(bill_content)
    qr.make(fit=True)

    img = qr.make_image(fill_color="black", back_color="white")
    
    # Save the QR code as an image file
    img.save("bill_qr.png")

    # Display the QR code in a new window
    qr_window = Toplevel()
    qr_window.title("Bill QR Code")

    qr_image = Image.open("bill_qr.png")
    qr_photo = ImageTk.PhotoImage(qr_image)
    qr_label = Label(qr_window, image=qr_photo)
    qr_label.image = qr_photo
    qr_label.pack()

    # Remove the temporary image file after displaying
    os.remove("bill_qr.png")
def GenerateBill():
    global randomRef
    bill_window = Toplevel(root)
    bill_window.title("Bill")

    # Create the bill layout
    bill_text = Text(bill_window)
    bill_text.pack()

    # Generate and format the bill
    bill = "-------- Cafe Bill --------\n\n"
    bill += f"Order No.: {randomRef}\n"
    bill += f"Date & Time: {localtime}\n\n"
    bill += "Items:\n"

    items = [
        ("French Fries", Fries.get(), 25),
        ("Lunch", Largefries.get(), 40),
        ("Burger", Burger.get(), 35),
        ("Pizza", Filet.get(), 50),
        ("Cheese Burger", Cheese_burger.get(), 30),
        ("Drinks", Drinks.get(), 35),
    ]

    total_price = 0
    for item, quantity, price in items:
        if quantity:
            total_price += float(quantity) * price
            bill += f"{item}: {quantity} x {price} = {float(quantity) * price}\n"

    bill += f"\nTotal Cost: Rs. {total_price:.2f}\n"
    bill += f"Service Charge: Rs. {float(total_price) / 99:.2f}\n"
    bill += f"Tax: Rs. {total_price * 0.33:.2f}\n"
    bill += f"Subtotal: Rs. {total_price:.2f}\n"
    bill += f"Overall Total: Rs. {total_price + float(total_price) / 99 + total_price * 0.33:.2f}\n"

    bill_text.insert(INSERT, bill)
def Ref():
    try:
        cof = float(Fries.get())
        colfries = float(Largefries.get())
        cob = float(Burger.get())
        cofi = float(Filet.get())
        cochee = float(Cheese_burger.get())
        codr = float(Drinks.get())

        costoffries = cof * 25
        costoflargefries = colfries * 40
        costofburger = cob * 35
        costoffilet = cofi * 50
        costofcheeseburger = cochee * 30
        costofdrinks = codr * 35

        costofmeal = "Rs.", str('%.2f' % (costoffries + costoflargefries + costofburger + costoffilet + costofcheeseburger + costofdrinks))
        PayTax = ((costoffries + costoflargefries + costofburger + costoffilet + costofcheeseburger + costofdrinks) * 0.33)
        Totalcost = (costoffries + costoflargefries + costofburger + costoffilet + costofcheeseburger + costofdrinks)
        Ser_Charge = ((costoffries + costoflargefries + costofburger + costoffilet + costofcheeseburger + costofdrinks) / 99)
        Service = "Rs.", str('%.2f' % Ser_Charge)
        OverAllCost = "Rs.", str(PayTax + Totalcost + Ser_Charge)
        PaidTax = "Rs.", str('%.2f' % PayTax)

        Service_Charge.set(Service)
        cost.set(costofmeal)
        Tax.set(PaidTax)
        Subtotal.set(costofmeal)
        Total.set(OverAllCost)
    except ValueError:
        messagebox.showerror("Error", "Invalid Input. Please enter numeric values.")


def qexit():
    root.destroy()

def reset():
    rand.set("")
    Fries.set("")
    Largefries.set("")
    Burger.set("")
    Filet.set("")
    Subtotal.set("")
    Total.set("")
    Service_Charge.set("")
    Drinks.set("")
    Tax.set("")
    cost.set("")
    Cheese_burger.set("")


#---------------------------------------------------------------------------------------
rand = StringVar()
Fries = StringVar()
Largefries = StringVar()
Burger = StringVar()
Filet = StringVar()
Subtotal = StringVar()
Total = StringVar()
Service_Charge = StringVar()
Drinks = StringVar()
Tax = StringVar()
cost = StringVar()
Cheese_burger = StringVar()


lblreference = Label(f1, font=( 'aria' ,16, 'bold' ),text="Order No.",fg="green",bd=20,anchor='w')
lblreference.grid(row=0,column=0)
txtreference = Entry(f1,font=('ariel' ,16,'bold'), textvariable=rand , bd=6,insertwidth=6,bg="blue" ,justify='right')
txtreference.grid(row=0,column=1)

lblfries = Label(f1, font=( 'aria' ,16, 'bold' ),text=" French Fries ",fg="blue",bd=10,anchor='w')
lblfries.grid(row=2,column=0)
txtfries = Entry(f1,font=('ariel' ,16,'bold'), textvariable=Fries , bd=6,insertwidth=4,bg="gray" ,justify='right')
txtfries.grid(row=2,column=1)

lblLargefries = Label(f1, font=( 'aria' ,16, 'bold' ),text="Lunch ",fg="blue",bd=10,anchor='w')
lblLargefries.grid(row=3,column=0)
txtLargefries = Entry(f1,font=('ariel' ,16,'bold'), textvariable=Largefries , bd=6,insertwidth=4,bg="gray" ,justify='right')
txtLargefries.grid(row=3,column=1)


lblburger = Label(f1, font=( 'aria' ,16, 'bold' ),text="Burger ",fg="blue",bd=10,anchor='w')
lblburger.grid(row=4,column=0)
txtburger = Entry(f1,font=('ariel' ,16,'bold'), textvariable=Burger , bd=6,insertwidth=4,bg="gray" ,justify='right')
txtburger.grid(row=4,column=1)

lblFilet = Label(f1, font=( 'aria' ,16, 'bold' ),text="Pizza ",fg="blue",bd=10,anchor='w')
lblFilet.grid(row=5,column=0)
txtFilet = Entry(f1,font=('ariel' ,16,'bold'), textvariable=Filet , bd=6,insertwidth=4,bg="gray" ,justify='right')
txtFilet.grid(row=5,column=1)

lblCheese_burger = Label(f1, font=( 'aria' ,16, 'bold' ),text="Cheese burger",fg="blue",bd=10,anchor='w')
lblCheese_burger.grid(row=6,column=0)
txtCheese_burger = Entry(f1,font=('ariel' ,16,'bold'), textvariable=Cheese_burger , bd=6,insertwidth=4,bg="gray" ,justify='right')
txtCheese_burger.grid(row=6,column=1)

#--------------------------------------------------------------------------------------
lblDrinks = Label(f1, font=( 'aria' ,16, 'bold' ),text="Drinks",fg="blue",bd=10,anchor='w')
lblDrinks.grid(row=1,column=0)
txtDrinks = Entry(f1,font=('ariel' ,16,'bold'), textvariable=Drinks , bd=6,insertwidth=4,bg="gray" ,justify='right')
txtDrinks.grid(row=1,column=1)

lblcost = Label(f1, font=( 'aria' ,16, 'bold' ),text="Cost",fg="black",bd=10,anchor='w')
lblcost.grid(row=2,column=2)
txtcost = Entry(f1,font=('ariel' ,16,'bold'), textvariable=cost , bd=6,insertwidth=4,bg="white" ,justify='right')
txtcost.grid(row=2,column=3)

lblService_Charge = Label(f1, font=( 'aria' ,16, 'bold' ),text="Service Charge",fg="black",bd=10,anchor='w')
lblService_Charge.grid(row=3,column=2)
txtService_Charge = Entry(f1,font=('ariel' ,16,'bold'), textvariable=Service_Charge , bd=6,insertwidth=4,bg="white" ,justify='right')
txtService_Charge.grid(row=3,column=3)

lblTax = Label(f1, font=( 'aria' ,16, 'bold' ),text="Tax",fg="black",bd=10,anchor='w')
lblTax.grid(row=4,column=2)
txtTax = Entry(f1,font=('ariel' ,16,'bold'), textvariable=Tax , bd=6,insertwidth=4,bg="white" ,justify='right')
txtTax.grid(row=4,column=3)

lblSubtotal = Label(f1, font=( 'aria' ,16, 'bold' ),text="Subtotal",fg="black",bd=10,anchor='w')
lblSubtotal.grid(row=5,column=2)
txtSubtotal = Entry(f1,font=('ariel' ,16,'bold'), textvariable=Subtotal , bd=6,insertwidth=4,bg="white" ,justify='right')
txtSubtotal.grid(row=5,column=3)

lblTotal = Label(f1, font=( 'aria' ,16, 'bold' ),text="Total",fg="green",bd=10,anchor='w')
lblTotal.grid(row=6,column=2)
txtTotal = Entry(f1,font=('ariel' ,16,'bold'), textvariable=Total , bd=6,insertwidth=4,bg="grey" ,justify='right')
txtTotal.grid(row=6,column=3)



def price():
    roo = Tk()
    roo.geometry("600x220+0+0")
    roo.title("Price List")
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="ITEM", fg="black", bd=5)
    lblinfo.grid(row=0, column=0)
    lblinfo = Label(roo, font=('aria', 15,'bold'), text="_____________", fg="white", anchor=W)
    lblinfo.grid(row=0, column=2)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="PRICE", fg="black", anchor=W)
    lblinfo.grid(row=0, column=3)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="French Fries", fg="steel blue", anchor=W)
    lblinfo.grid(row=1, column=0)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="25", fg="steel blue", anchor=W)
    lblinfo.grid(row=1, column=3)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="Lunch ", fg="steel blue", anchor=W)
    lblinfo.grid(row=2, column=0)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="40", fg="steel blue", anchor=W)
    lblinfo.grid(row=2, column=3)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="Burger ", fg="steel blue", anchor=W)
    lblinfo.grid(row=3, column=0)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="35", fg="steel blue", anchor=W)
    lblinfo.grid(row=3, column=3)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="Pizza ", fg="steel blue", anchor=W)
    lblinfo.grid(row=4, column=0)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="50", fg="steel blue", anchor=W)
    lblinfo.grid(row=4, column=3)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="Cheese Burger", fg="steel blue", anchor=W)
    lblinfo.grid(row=5, column=0)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="30", fg="steel blue", anchor=W)
    lblinfo.grid(row=5, column=3)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="Drinks", fg="steel blue", anchor=W)
    lblinfo.grid(row=6, column=0)
    lblinfo = Label(roo, font=('aria', 15, 'bold'), text="35", fg="steel blue", anchor=W)
    lblinfo.grid(row=6, column=3)

    roo.mainloop()

def open_main_application():
    rand.set("")
    Fries.set("")
    Largefries.set("")
    Burger.set("")
    Filet.set("")
    Subtotal.set("")
    Total.set("")
    Service_Charge.set("")
    Drinks.set("")
    Tax.set("")
    cost.set("")
    Cheese_burger.set("")
    root.deiconify()

def login():
    username = username_entry.get()
    password = password_entry.get()

    if username == "admin" and password == "0000":  # Replace with your desired login credentials
        login_window.destroy()
        open_main_application()
    else:
        messagebox.showerror("Login Failed", "Invalid username or password")
# Create a Treeview for Order History
order_tree = ttk.Treeview(root, columns=("Order ID", "Order Date", "Total Amount"))
order_tree.heading("#0", text="Order ID")
order_tree.heading("Order ID", text="Order ID")
order_tree.heading("Order Date", text="Order Date")
order_tree.heading("Total Amount", text="Total Amount")
order_tree.pack()
saved_orders = []

# Function to save order to the list
def SaveOrderHistory():
    order_data = {
        "Order ID": rand.get(),
        "Order Date": localtime,
        "Order Time": time.strftime("%H:%M:%S"),
        "French Fries": Fries.get(),
        "Lunch": Largefries.get(),
        "Burger": Burger.get(),
        "Pizza": Filet.get(),
        "Cheese Burger": Cheese_burger.get(),
        "Drinks": Drinks.get(),
        "Subtotal": Subtotal.get(),
        "Tax": Tax.get(),
        "Service Charge": Service_Charge.get(),
        "Total": Total.get()
    }
    saved_orders.append(order_data)


# Function to show saved orders
def show_saved_orders():
    orders_window = Toplevel(root)
    orders_window.title("Order History")

    order_tree = ttk.Treeview(orders_window, columns=list(saved_orders[0].keys()), show="headings")

    for column in saved_orders[0].keys():
        order_tree.heading(column, text=column)
        order_tree.column(column, width=100, anchor=CENTER)

    for i, order_data in enumerate(saved_orders, start=1):
        values = [order_data[column] for column in saved_orders[0].keys()]
        order_tree.insert("", i, values=values)

    order_tree.pack(expand=YES, fill=BOTH)


# Create a login window
login_window = Toplevel(root)
login_window.title("Login")
login_window.geometry("400x300+300+200")
username_label = Label(login_window, text="Username:")
username_label.grid(row=0, column=0, padx=10, pady=10)
username_entry = Entry(login_window)
username_entry.grid(row=0, column=1, padx=10, pady=10)

password_label = Label(login_window, text="Password:")
password_label.grid(row=1, column=0, padx=10, pady=10)
password_entry = Entry(login_window, show="*")  # Use show="*" to hide the password
password_entry.grid(row=1, column=1, padx=10, pady=10)

login_button = Button(login_window, text="Login", command=login)
login_button.grid(row=2, columnspan=2, pady=10)

#-----------------------------------------buttons------------------------------------------
lblTotal = Label(f1,text="---------------------",fg="white")
lblTotal.grid(row=7,columnspan=3)
# Button to calculate total cost
btnTotal = Button(f1, padx=10, pady=6, bd=6, fg="black", font=('ariel', 16, 'bold'), width=8, text="TOTAL", bg="green", command=Ref)
btnTotal.grid(row=8, column=0, padx=2, pady=2)

# Button to reset the form
btnReset = Button(f1, padx=10, pady=6, bd=6, fg="black", font=('ariel', 16, 'bold'), width=8, text="RESET", bg="yellow", command=reset)
btnReset.grid(row=8, column=1, padx=2, pady=2)

# Button to exit the application
btnExit = Button(f1, padx=10, pady=6, bd=6, fg="black", font=('ariel', 16, 'bold'), width=8, text="EXIT", bg="red", command=qexit)
btnExit.grid(row=8, column=2, padx=2, pady=2)

# Button to show price list
btnPrice = Button(f1, padx=10, pady=6, bd=6, fg="black", font=('ariel', 16, 'bold'), width=8, text="PRICE", bg="green", command=price)
btnPrice.grid(row=8, column=3, padx=2, pady=2)

# Button to generate bill
btnGenerateBill = Button(f1, padx=10, pady=6, bd=6, fg="black", font=('ariel', 12, 'bold'), width=8, text="Bill", bg="purple", command=GenerateBill)
btnGenerateBill.grid(row=8, column=4, padx=2, pady=2, sticky="e")

# Button to generate QR code
btnGenerateQR = Button(f1, padx=10, pady=6, bd=6, fg="black", font=('ariel', 12, 'bold'), width=8, text="QR Code", bg="purple", command=GenerateBill0)
btnGenerateQR.grid(row=8, column=5, padx=2, pady=2, sticky="e")

# Button to save order and view order history
btnSaveOrder = Button(f1, padx=10, pady=6, bd=6, fg="black", font=('ariel', 12, 'bold'), width=8, text="Save", bg="orange", command=SaveOrderHistory)
btnSaveOrder.grid(row=8, column=6, padx=2, pady=2, sticky="e")

# Button to view saved orders
btnViewOrders = Button(f1, padx=10, pady=6, bd=6, fg="black", font=('ariel', 12, 'bold'), width=8, text="Orders", bg="purple", command=show_saved_orders)
btnViewOrders.grid(row=8, column=7, padx=2, pady=2, sticky="e")



root.iconify()
root.mainloop()
