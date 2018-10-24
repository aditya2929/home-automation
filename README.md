# home-automation
#include<reg51.h>
#define data P1
char a[4];
char t;
sbit FAN1=P2^4;
sbit LAMP2=P2^5;
sbit TV3=P2^6;
sbit TV4=P2^7;
sbit rs=P2^1;
sbit rw=P2^2;
sbit en=P2^3;
void lcd_string(char a[]);
void lcd_data(char);
void lcd_command(char cmd);
void delay(int k);
void passwordrecv(void)
{
int i;
	for(i=0;i<4;i++)
{
	while(RI==0);
	a[i]=SBUF;
	SBUF=a[i];
		RI=0;
}
}
void recv(void)
{
while(RI==0);
t=SBUF;
RI=0;
}
void main(void)
	{
		SCON=0X50;
		TMOD=0X20;
		TH1=0XFD;
		TR1=1;
		lcd_command(0x02);
		lcd_command(0x80);
		lcd_command(0x0c);
		
		
		while(1)
		{
		lcd_string("pls enter the password");
		passwordrecv();
		lcd_command(0x01);
			if(a[0]=='A'&&a[1]=='B'&&a[2]=='C'&&a[3]=='D')
			{
		
				lcd_command(0x01);
	  lcd_string("password is access");
	  	while(1){
			recv();
		if(t=='1')
	 {
	 lcd_command(0x01);
	  lcd_string("fan is on");
		 FAN1=0;
	 }
			if(t=='2')
	 {
	 lcd_command(0x01);
	  lcd_string("fan is off");
		 FAN1=1;
	 }
	 if(t=='3')
	 {
	 lcd_command(0x01);
	  lcd_string("lamp is on");
		 LAMP2=0;
	 }
	 if(t=='4')
	 {
	 lcd_command(0x01);
	  lcd_string("lamp is off");
		 LAMP2=1;
	 }
	 if(t=='5')
	 {
	 lcd_command(0x01);
	  lcd_string("tv is on");
		 TV3=0;
	 }
	 	 if(t=='6')
	 {
	 lcd_command(0x01);
	  lcd_string("tv is off");
		 TV3=1;
	 }
	 	 if(t=='7')
	 {
	 lcd_command(0x01);
	  lcd_string("tv is off");
		 TV4=0;
	 }
	 	 if(t=='8')
	 {
	 lcd_command(0x01);
	  lcd_string("tv is off");
		 TV4=1;
	 }
	 if(t=='0')
	 {
		 lcd_command(0x01);
	  lcd_string("all is off");
		 FAN1=LAMP2=TV3=TV4=1;
	 }
	 if(t=='9')
	 {
		 lcd_command(0x01);
	  lcd_string("all is on");
		  FAN1=LAMP2=TV3=TV4=0;
		  }
		  if(t=='q')
		  {
		  
		  lcd_command(0x01);
			  lcd_string("exiting");
			  delay(5);
			  lcd_command(0x01);
			  break;
		  }
		  }
	 }
	 }
	 }				 
	void lcd_string(char a[])
	{
		int i;
		lcd_command(0x80);
		for(i=0;a[i]!='\0';i++)
		{
			lcd_data(a[i]);
			if(i==15)
			{
			lcd_command(0xC0);
			}
		}
	}
			void lcd_command(char cmd)
		{
			data=cmd;
			rs=0;
			rw=0;
			en=1;
			delay(3);
			en=0;
		}
		void lcd_data(char dat)
		{
			data=dat;
			rs=1;
			rw=0;
			en=1;
			delay(3);
			en=0;
		}
	
		void delay(int k)
{
	int g,h;
	for(g=0;g<k;g++)
	for(h=0;h<10000;h++);
} 
