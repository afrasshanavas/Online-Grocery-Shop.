# Online-Grocery-Shop.
Online grocery Shop in C++
#include<fstream.h>
#include<conio.h>
#include<stdio.h>
#include<string.h>
#include<process.h>
#include<ctype.h>
#include<graphics.h>
#include<stdlib.h>
#include<dos.h>
void graphics();
void addcart();
void custview();
char p[100];int q;
char user[100];
int a=0;
void graphics()
   {
    int i,gd=DETECT,gm;
    initgraph(&gd,&gm,"..\\bgi");
    {
     setcolor(WHITE);
     for(i=10;i<555;i++)
      line(25+i,170,25+i,200);
     for(i=10;i<555;i++)
     {
      setcolor(GREEN);
      line(25+i,170,25+i,200);
      delay(5);
      setcolor(WHITE);
      settextstyle(SMALL_FONT,HORIZ_DIR,16);
      outtextxy(220,220,"LOADING");
      delay(5);
      setcolor(rand()%16);
      settextstyle(SMALL_FONT,HORIZ_DIR,16);
      outtextxy(390,220,".....");
     }

    }closegraph();
}
class admin
   {
      char name[100],type[100];
      float price,discount,netamt;
    public:
      void enterdata()
	  {
	    cout<<"\nEnter the category of item:";
	    gets(type);
	    cout<<"\nEnter the product name:";
	    gets(name);
	    cout<<"\nEnter the product price:";
	    cin>>price;
	  }
      char* getname()
	  {
	    return name;
	  }
      void displaydata()
	  {
	    cout<<"\n"<<name<<"\t\t"<<type<<"\t\t"<<price<<"\n";
	  }
      void modifyname(char newname[])
	  {
	    strcpy(name,newname);
	  }
      void modifyprice(float newprice)
	  {
	    price=newprice;
	  }
      char* rettype()
	  {
	    return type;
	  }
      float retprice()
	  {
	    return price;
	  }
      void add();
      void search();
      void modify();
      void removep();
      void view();
    }A;
void adminmenu();
void customermenu();
void updatename()
	{
	  fstream f3("admin.dat",ios::in|ios::out|ios::binary);
	  char p[100];
	  cout<<"\nEnter the pdt name to modify:";
	  gets(p);
	  char m[100];
	  cout<<"\nEnter the modified name:";
	  gets(m);
	  int a=0;
	  int flag=0;
	  while(f3.read((char *)&A,sizeof(A)))
	     { a++;
	       if(strcmpi(p,A.getname())==0)
		  {
		    flag=1;
		    f3.seekp(f3.tellp()-sizeof(A));
		    A.modifyname(m);
		    f3.write((char*)&A,sizeof(A));
		   cout<<"\nProduct name successfully updated";
		  }
	     }
	  if(flag==0)
	   cout<<"\n Product not found";
	   f3.close();
      }
void updateprice()
      {
	  fstream f4("admin.dat",ios::in|ios::out|ios::binary);
	  char n[100];
	  cout<<"\nEnter the name to modify:";
	  gets(n);
	  float p;
	  cout<<"\nEnter the modified price:";
	  cin>>p;
	  int flag=0;int a=0;
	  while(f4.read((char*)&A,sizeof(A)))
	     {
	       a++;
	       if(strcmpi(A.getname(),n)==0)
		{
		  flag=1;
		  f4.seekp(f4.tellp()-sizeof(A));
		  A.modifyprice(p);
		  f4.write((char*)&A,sizeof(A));
		  cout<<"\nProduct price successfully updated";
		}
	     }
		if(flag==0)
	   cout<<"\nProduct not found";
	   f4.close();
      }
void admin::add()
      {
	 fstream f1("admin.dat",ios::binary|ios::app);
	 int n,flag=0;
	 char s;
	 do{
	     a++;
	     A.enterdata();
	     f1.write((char*)&A,sizeof(A));
	     flag=1;
	     cout<<"\nDo you want to add more products(press y):";
	     cin>>s;
	    }while(s=='y'||s=='Y');
	if(flag==1)
	   cout<<"\nProducts successfully added";
	f1.close();
	adminmenu();
      }
void admin::search()
      {
	ifstream f2("admin.dat",ios::binary);
	char s[100];
	int flag=0;int a=0;
	cout<<"\nEnter the product to search:";
	gets(s);
	while(f2.read((char*)&A,sizeof(A)))
	   {
	     a++;
	     if(strcmpi(A.getname(),s)==0)
	       {
		A.displaydata();
		flag=1;
	       }
	  }
       if(flag==0)
	 cout<<"\nSorry,Product currently out of stock";
       else if(flag==1)
	 cout<<"\nProduct successfully found";
       f2.close();
       adminmenu();
   }
void admin::modify()
   {
     char c;
    p: cout<<"\n_________________________________________________________________________";
     cout<<"\n                           (a)Modify name                       ";
     cout<<"\n                           (b)Modify price                     ";
     cout<<"\n                           (c)Return to admin menu                       ";
     cout<<"\n-------------------------------------------------------------------------";
     cout<<"\n                           Enter your choice:";
     cin>>c;
     if(c=='a')
	{
	 updatename();
	}
     else if(c=='b')
	 {
	  updateprice();
	 }
     else if(c=='c')
	 adminmenu();
     else
      {
	cout<<"\nSorry wrong choice";
	cout<<"\nEnter again";
	goto p;
      }
     adminmenu();
   }
void admin::removep()
   {
     ifstream f4("admin.dat",ios::binary);
     ofstream fo4("temp.dat",ios::binary);
     char a[100];
     cout<<"\nEnter the name of the product to delete:";
     gets(a);int b=0;
     int flag=0;
     while(f4.read((char*)&A,sizeof(A)))
      { b++;
	if(strcmpi(A.getname(),a)==0)
	  flag=1;
	if(strcmpi(A.getname(),a)!=0)
	  fo4.write((char*)&A,sizeof(A));
      }
    if(flag==0)
      cout<<"\nProduct not available";
    else
       cout<<"\nProduct removed";
    f4.close();
    fo4.close();
    remove("admin.dat");
    rename("temp.dat","admin.dat");
    adminmenu();
   }
void admin::view()
  {
    ifstream f5("admin.dat",ios::binary);
    cout<<"\nNAME\tCATEGORY\tPRICE\n";
    int a=0;
    while(f5.read((char*)&A,sizeof(A)))
      {
	A.displaydata();
	a++;
      }
    f5.close();
    adminmenu();
  }
void mainmenu()
  {
      int ch;
      clrscr();
      cout<<"\n                                   BB                                                              ";
      cout<<"\n                        *=*=*Lets Get Started!!*=*=*                       ";
      cout<<"\n                                                                           ";
      cout<<"\n***************************************************************************";
      cout<<"\n                               MAIN MENU                                     ";
      cout<<"\n                         1.Admin menu                                      ";
      cout<<"\n                         2.Customer menu                                   ";
      cout<<"\n                         3.Exit					          ";
      cout<<"\n***************************************************************************";
      cout<<"\n                         Enter your choice:";
      cin>>ch;
      if(ch==1)
	{
	 char d[100];
	 char username[100];
	 clrscr();
	n: cout<<"\nEnter the username:";
	 gets(username);
	 cout<<endl;
	 cout<<"\nEnter the password:";
	 for(int y=0;y<3;y++)
	       {
		 d[y]=getch();
		 cout<<"*";
	       }
	     d[y]='\0';
	 char b;
	 int ch1;
	 if(d[0]=='a'&&d[1]=='d'&&d[2]=='m')
	   {
	     cout<<"\nPassword correct";
	     adminmenu();
	   }
	 else
	   {
	    cout<<"\nIncorrect password";
	    cout<<"\nTry again";
	    goto n;
	   }
	}
      else if(ch==2)
       {
	customermenu();
       }
      else if(ch==3)
	exit(0);
   }

void adminmenu()
  {
     int ch1;
	      o: cout<<"\n                           BB                              ";
		 cout<<"\n***********************************************************";
		 cout<<"\n                   *-*-*ADMIN MENU*-*-*                    ";
		 cout<<"\n                  1.To add new products                    ";
		 cout<<"\n                  2.To search for a product                ";
		 cout<<"\n                  3.To modify details of a product    ";
		 cout<<"\n                  4.To remove a product                    ";
		 cout<<"\n                  5.View product details                   ";
		 cout<<"\n                  6.Return to main menu                    ";
		 cout<<"\n***********************************************************";
		 cout<<"\n                 Enter your choice:";
		 cin>>ch1;
		 if(ch1==1)
		   A.add();
		 else if(ch1==2)
		 {
		   cout<<"\nNAME\t\tCATEGORY\t\tPRICE\n";
		   A.search();
		 }
		 else if(ch1==3)
		   A.modify();
		 else if(ch1==4)
		   A.removep();
		 else if(ch1==5)
		   A.view();
		 else if(ch1==6)
		   mainmenu();
		 else
		   {
		     cout<<"\nSorry wrong choice";
		     cout<<"\nTry again";
		     goto o;
		   }
  }
class customer
   {
     public:
     char cname[100],cadd[100],email[100],password[10],cmode[100],pdts[100];
     char phone[100];int qty;
     float pr,tamount,gtotal;
     char *getpdts()
	  {return pdts;
	  }
     int gettamount()
	{
	 return tamount;
	}
     void customerentry()
	{
	  cout<<"\nEnter your username:";
	  gets(cname);
	  cout<<"\nEnter your address:";
	  gets(cadd);
	  cout<<"\nEnter your email id:";
	  gets(email);
	  cout<<"\nEnter your phone number:";
	  cin>>phone;
	  cout<<"\nCreate your password(7 characters):";
	  for(int i=0;i<7;i++)
	   {
	     password[i]=getch();
	     cout<<"*";
	   }
	  password[i]='\0';
	}
     char *getcustomer()
	{
	  return cname;
	}
     char*retpass()
	{
	  return password;
	}
     void modad(char address[])
	{
	  strcpy(cadd,address);
	 }
     void resetpass(char pass[])
	 {
	  strcpy(password,pass);
	 }
     void tamt(int a)
	 {
	  tamount=a*qty;
	  }
     void viewcart()
	 {
	  cout<<"\n"<<pdts<<"   "<<pr<<"    "<<qty<<"   "<<tamount;
	 }
     void accinfo()
	 {
	  cout<<"\nUSER NAME:"<<cname;
	  cout<<"\nADDRESS  :"<<cadd;
	  cout<<"\nPHONE NO :"<<phone;
	  cout<<"\nEMAIL ID :"<<email;
	 }
     void payment();
     void cartentry();
     void pr1(int a)
	{
	  pr=a;
	}
  }C;
customer C1;

void customer::payment()
     {
	clrscr();
	cout<<"\n$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$";
	cout<<"\n                 PAYMENT DETAILS                       ";
	cout<<"\n                ------------------                     ";
	cout<<"\n               1.CARD                                  ";
	cout<<"\n               2.CASH ON DELIVERY                      ";
	cout<<"\n               3.Go back                               ";
	cout<<"\n$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$";
	cout<<"\nENTER THE MODE OF PAYMENT:";
	int mp;
	cin>>mp;
	if(mp==1)
	  {
	    long int cardno;
	    int month,year,cvv;
	    ifstream v("list.dat",ios::binary);
	    long int x=0;
	    cout<<"\nEnter the card number:";
	    cin>>cardno;
	    cout<<"\nEnter the expiry date of the card:";
	    cout<<"\nMonth:";
	    cin>>month;
	    cout<<"\nYear:";
	    cin>>year;
	    cout<<"\nEnter the cvv:";
	    cin>>cvv;
	    cout<<"\n\n\n\n";
	    cout<<"\n          PROCEEDING TO BILL....... ";
	    cout<<"\n  (This will take a few moments please wait.......)";
	    sleep(5);
	    graphics();
	    cout<<"\n          YOUR ORDER HAS BEEN PROCESSED!!!         ";
	    while(v.read((char*)&C1,sizeof(C1)))
	     {
	       C1.viewcart();
	       x+=C1.gettamount();
	      }
	    cout<<"\n       YOUR PRODUCT WILL BE DELIVERED IN 10 MIN    ";
	    cout<<"\n\n          Thankyou for shopping with BigBasket.....               ";
	  }
	else if(mp==2)
	  {
	    ifstream v("list.dat",ios::binary);
	    long int x=0;
	    cout<<"\n\n\n\n";
	    cout<<"\n          PROCEEDING TO BILL....... ";
	    cout<<"\n  (This will take a few moments please wait.......)";
	    sleep(5);
	    graphics();
	    cout<<"\n          YOUR ORDER HAS BEEN PROCESSED!!!         ";
	    while(v.read((char*)&C1,sizeof(C1)))
	     {
	       C1.viewcart();
	       x+=C1.gettamount();
	     }
	    cout<<"\n       YOUR PRODUCT WILL BE DELIVERED IN 10 MIN    ";
	    cout<<"\n\n        Thankyou for shopping with BigBasket.....";
	   }
	else if(mp==3)
	   custview();
	else
	   cout<<"\nSorry wrong choice";
      }
void customer::cartentry()
    {
      cout<<"\nTo add the product to cart enter the product name:";
      gets(pdts);
      cout<<"\nEnter the quantity:";
      cin>>qty;
    }

void customermenu()
   {
    int cust; char c;
    c:	clrscr();
	cout<<"\n                                 BB                                        ";
	cout<<"\n***************************************************************************";
	cout<<"\n                     *-*-*-*CUSTOMER MENU*-*-*-*                           ";
	cout<<"\n                          1.Sign in                                        ";
	cout<<"\n                          2.Login                                          ";
	cout<<"\n                          3.Return to main menu                            ";
	cout<<"\n***************************************************************************";
	cout<<"\n                     Enter your choice:";
	cin>>cust;
	if(cust==1)
	   {
	    fstream fc("customer.dat",ios::binary|ios::app);
	    cout<<"\n*-*-*ENTER DETAILS*-*-*";
	    C.customerentry();
	    fc.write((char*)&C,sizeof(C));
	    cout<<"\nSign in Success, press any key to continue\n";
	    getch();
	    fc.close();
	    goto c;
	   }
	else if(cust==2)
	    {
	      ifstream P("customer.dat",ios::binary);
	      char pass[100];
	      int flag=0;
	      cout<<"\n*********************Welcome dear customer!!**************************";
	      cout<<"\n\nEnter your username:";
	      gets(user);
	      p:cout<<"\nEnter your password:";
	      for(int y=0;y<7;y++)
	       {
		 pass[y]=getch();
		 cout<<"*";
	       }
	       pass[y]='\0';
	      cout<<endl;
	      getch();
	      cout<<"\n**********************************************************************";
	      cout<<endl;
	      while(P.read((char*)&C,sizeof(C)))
	       {
		if(strcmp(user,C.getcustomer())==0&& strcmp(pass,C.retpass())==0)
		  {
		    flag=1;
		    cout<<"\n Log in success!!!\nPress enter key to continue";
		    getch();
		    graphics();
		    custview();
		    getch();
		    cout<<"\nPress any key to continue to go back:";
		    getch();
		    C.payment();
		    //break;
		  }
		else
		 flag=-1;
	      }
	    if(flag==-1)
	      {
		   char c;
		   cout<<"\nSorry,Wrong password";
		   cout<<"\nTo try again press 'Y':";
		   cin>>c;
		   if(c=='Y'||c=='y')
		    goto p;
		  else
		   customermenu();
	      }
	    P.close();

	}
     else if(cust==3)
       mainmenu();
  }

void custview()
  {
   clrscr();
   char *n=NULL;
   strcpy(n,user);

 cm: cout<<"\n___________________________________________________________________________";
     cout<<"\n                                   BB                                      ";
     cout<<"\n    *********************************************************************  ";
     cout<<"\n                            1.Settings                                     ";
     cout<<"\n                            2.Proceed to shop                              ";
     cout<<"\n                            3.Log out                                      ";
     cout<<"\n    *********************************************************************  ";
     cout<<"\n___________________________________________________________________________";
     cout<<"\n                          Enter your choice:";
     int  setch;
     cin>>setch;
     if(setch==1)
       {
	mm:cout<<"\n *********************************************************************  ";
	cout<<"\n                            1.Modify address                               ";
	cout<<"\n                            2.Reset password                               ";
	cout<<"\n                            3.Account Info                                 ";
	cout<<"\n                            4.Go back                                      ";
	cout<<"\n    *********************************************************************  ";
	cout<<"\n___________________________________________________________________________";
	cout<<"\n                          Enter your choice:";
	int setchch;
	cin>>setchch;

	if(setchch==1)
	  {
	   fstream a("customer.dat",ios::binary|ios::in|ios::out);
	   int flag=0;
	   while(a.read((char*)&C,sizeof(C)))
	    {
	      if(strcmp(n,C.getcustomer())==0)
		{
		  cout<<"\nEnter your new address:";
		  char modad[100];
		  gets(modad);
		  a.seekp(a.tellp()-sizeof(C));
		  C.modad(modad);
		  a.write((char*)&C,sizeof(C));
		  flag=1;
		}
	     }
	     a.close();
	    if(flag==1)
	     cout<<"\nAddress successfully modified";
	   goto mm;
	  }
       else if(setchch==2)
	{
	 fstream b("customer.dat",ios::binary|ios::in|ios::out);
	 int flag=0;
	 cout<<"\nEnter your current password:";
	 char pass[100];
	 gets(pass);

	 while(b.read((char*)&C,sizeof(C)))
	   {

	    char newpass[100];

	    if(strcmp(pass,C.retpass())==0)
	     {
	      cout<<"\nEnter your new password:";
	      gets(newpass);
	      b.seekg(b.tellp()-sizeof(C));
	      C.resetpass(newpass);
	      b.write((char*)&C,sizeof(C));
	      flag=1;
	     }
	    }
	    b.close();
	  if(flag==1)
	    cout<<"\nPassword successfully updated!!";
	  goto mm;
	 }
       else if(setchch==3)
	{
	 C.accinfo();
	 goto mm;
	}
       else if(setchch==4)
	{
	  goto cm;
	}
      }
    else if(setch==2)
      {
	fstream a1("list.dat",ios::binary|ios::out);
	a1.close();
	clrscr();
	a:
       cout<<"\n                                   BB                                      ";
       cout<<"\n***************************************************************************";
       cout<<"\n                       *-*-*GET YOUR CART FILLED!!*-*-*                    ";
       cout<<"\n                                                                           ";
       cout<<"\n____________________________CHOOSE YOUR CATEGORY___________________________";
       cout<<"\n                            1.Fresh vegetables                             ";
       cout<<"\n                            2.Fresh fruits                                 ";
       cout<<"\n                            3.Dry fruits                                   ";
       cout<<"\n                            4.Household items                              ";
       cout<<"\n                            5.View cart                                     ";
       cout<<"\n                            6.Go back                                      ";
       cout<<"\n---------------------------------------------------------------------------";
       cout<<"\n                    What would you like to shop today??                    ";
       cout<<"\n                           Enter your choice:";
       int cat;
       cin>>cat;
       if(cat==1)
	{
	  fstream O("list.dat",ios::binary|ios::app);
	 cout<<"\n   VEGETABLES     PRICE      ";
	 ifstream FV("admin.dat",ios::binary);
	 while(FV.read((char*)&A,sizeof(A)))
	 {
	   if(strcmpi(A.rettype(),"vegetables")==0)
	    cout<<"\n   "<<A.getname()<<"   "<<A.retprice();
	 }
	FV.close();
	b:   C1.cartentry();
	int a=5;
	ifstream Fa("admin.dat",ios::binary);
	char *z;
	z=C1.getpdts();
	 while(Fa.read((char*)&A,sizeof(A)))
	 {  a=5;
	   if(strcmpi(A.getname(),z)==0)
	      break;
	   else
	       a=6;

	 }
	 if(a==6)
	   {
	    cout<<"\nSorry product out of stock!!!";
	    goto a;
	   }
	 C1.tamt(A.retprice());
	C1.pr1(A.retprice());
	O.write((char*)&C1,sizeof(C1));

	 cout<<"\nDo you want to add more products:";
	 char ch;
	 cin>>ch;
	 if(ch=='y'||ch=='Y')
	  goto b;
	 goto a;
	 Fa.close();
	 O.close();
       }
    else if(cat==2)
       {
	fstream O("list.dat",ios::binary|ios::app);
	cout<<"\n   FRUITS    PRICE    ";
	ifstream FF("admin.dat",ios::binary);
	 while(FF.read((char*)&A,sizeof(A)))
	 {
	   if(strcmpi(A.rettype(),"fruits")==0)
	    cout<<"\n   "<<A.getname()<<"   "<<A.retprice();
	 }
	 FF.close();
	 x:C1.cartentry();
	 int a=5;
	ifstream Fa("admin.dat",ios::binary);
	 while(Fa.read((char*)&A,sizeof(A)))
	 { a=5;
	   if(strcmpi(A.getname(),C1.getpdts())==0)
		break;
	   else
	       a=6;

	 }
	 if(a==6)
	   {
	    cout<<"\nSorry product out of stock!!!";
	    goto a;
	   }
	 C1.tamt(A.retprice());
	 C1.pr1(A.retprice());
	 O.write((char*)&C1,sizeof(C1));

	 cout<<"\nDo you want to add more products:";
	 char ch;
	 cin>>ch;
	 if(ch=='y'||ch=='Y')
	  goto x;
	 goto a;
	 Fa.close();
	 O.close();

       }
     else if(cat==3)
       {
	fstream O("list.dat",ios::binary|ios::app);
	cout<<"\n   DRY FRUITS    PRICE    ";
	ifstream DF("admin.dat",ios::binary);
	 while(DF.read((char*)&A,sizeof(A)))
	 {
	   if(strcmpi(A.rettype(),"dryfruits")==0)
	    cout<<"\n   "<<A.getname()<<"   "<<A.retprice();
	 }
	  DF.close();
	c:C1.cartentry();
	  int a=5;
	ifstream Fa("admin.dat",ios::binary);
	 while(Fa.read((char*)&A,sizeof(A)))
	 { a=5;
	   if(strcmpi(A.getname(),C1.getpdts())==0)
		break;
	   else
	       a=6;

	 }
	 if(a==6)
	   {
	    cout<<"\nSorry product out of stock!!!";
	    goto a;
	   }
	 C1.tamt(A.retprice());
	 C1.pr1(A.retprice());
	 O.write((char*)&C1,sizeof(C1));

	 cout<<"\nDo you want to add more products:";
	 char ch;
	 cin>>ch;
	 if(ch=='y'||ch=='Y')
	  goto c;

	  goto a;
	  Fa.close();
	 O.close();
       }
     else if(cat==4)
       {
	fstream O("list.dat",ios::binary|ios::app);
	cout<<"\n HOUSEHOLDS    PRICE    ";
	ifstream HH("admin.dat",ios::binary);
	 while(HH.read((char*)&A,sizeof(A)))
	 {
	   if(strcmpi(A.rettype(),"household")==0)
	    cout<<"\n   "<<A.getname()<<"   "<<A.retprice();
	 }
	 HH.close();
	 d:C1.cartentry();
	 int a=5;
	ifstream Fa("admin.dat",ios::binary);
	 while(Fa.read((char*)&A,sizeof(A)))
	 { a=5;
	   if(strcmpi(A.getname(),C1.getpdts())==0)
		break;
	   else
	       a=6;

	 }
	 C1.tamt(A.retprice());
	 if(a==6)
	   {
	    cout<<"\nSorry product out of stock!!!";
	    goto a;
	   }
	 C1.pr1(A.retprice());
	 O.write((char*)&C1,sizeof(C1));

	 cout<<"\nDo you want to add more products:";
	 char ch;
	 cin>>ch;
	 if(ch=='y'||ch=='Y')
	  goto d;
	 goto a;
	 Fa.close();
	 O.close();

       }
     else if(cat==5)
       {
	 clrscr();
	 long int x=0;
	 a1.close();
	 ifstream v("list.dat",ios::binary);
	 cout<<"\n+_+_+_+_+_+_+_+_+_+_+_+YOUR CART+_+_+_+_+_+_+_+_+_+_+_+";
	 cout<<"\n      ITEMS           PRICE      QUANTITY     TOTAL    ";
	 while(v.read((char*)&C1,sizeof(C1)))
	  {
	    C1.viewcart();
	    x+=C1.gettamount();
	  }
	 cout<<"\n-----------GRAND TOTAL=";
	 cout<<x;
	 cout<<"\n-----------------";
	 cout<<"\n+_+_+_+_+_+_+_+_+_+_+_+_+_+_+_+_+_+_+_+_+_+_+_++_+_+_+_";
	 cout<<"\nDo you want to proceed to bill?(press 1):";
	 int bi;
	 cin>>bi;
	 if(bi==1)
	   C.payment();
	 else
	   goto a;
	 v.close();
       }
     else if(cat==6)
	customermenu();
     }
  else if(setch==3)
     customermenu();
  else
   {
     cout<<"\nSorry wromg choice";
    custview();
   }
 }
void main()
  {
    clrscr();
    cout<<"\n___________________________________________________________________________";
    cout<<"\n                                                                           ";
    cout<<"\n                                                                           ";
    cout<<"\n                                                                           ";
    cout<<"\n                                                                           ";
    cout<<"\n                                                                           ";
    cout<<"\n           *-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*         ";
    cout<<"\n                         WELCOME TO BIGBASKET.COM                          ";
    cout<<"\n           *-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*-*         ";
    cout<<"\n                                                                           ";
    cout<<"\n                                                                           ";
    cout<<"\n                                                                           ";
    cout<<"\n                                                                           ";
    cout<<"\n                                                                           ";
    cout<<"\n___________________________________________________________________________";
    cout<<"\n                           || HAPPY SHOPPING||                             ";
    cout<<"\n                                                        Done by:           ";
    cout<<"\n                                                        Maria James Antony ";
    cout<<"\n                                                        Afras Shanavas  \n ";
    getch();

    mainmenu();
    getch();
  }
