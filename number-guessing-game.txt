#include<iostream>
#include<cmath>
using namespace std;

class solution{
    public:
    void guessnum();
    void hint(int num,int flag,int choice);
};

void solution :: guessnum()
{   int choice,limit,ans;
    cout<<"Select Difficulty for Guessing Number :\n1.Easy [1 digit]\n2.Medium [2 digit]\n3.Hard [3 digit]";
    cout<<"\nEnter your choice :";
    cin>>choice;
    if(choice==1)
        limit=10;
    else if(choice==2)
        limit=100;
    else if(choice==3)
        limit=1000;
    else
        cout<<"\nPlease enter a valid choice !";

    int num=rand()%limit;
    if(choice==2 && num<10)
        num=num+10;
    else if(choice==3 && num<100)
        num=num+100;

    int attempt=0;

    label:
    cout<<"\nEnter your answer :";
    cin>>ans;
    if(ans==num)
    {
        cout<<"Hooray ! Your guess is correct !\n\n";
        goto label2;
    }    
    else
    {   
        int diff;
        diff=abs(num-ans);
        int factor;
        factor=pow(choice,2);

        if(diff<=2*factor)
            cout<<"You are too close to answer !\n";
        else if(diff<=4*factor)
            cout<<"You are close to answer !\n";
        else if(diff<=6*factor)
            cout<<"You are far from answer !\n";
        else
            cout<<"You are too far from your answer!\n";
        
        attempt++;
        if((attempt==4 && choice==1) || (attempt==5 && choice==2) ||attempt==6)
        {   char ch;
            cout<<"\nSorry ,you have exceeded maximum attempts !";
            cout<<"\nThe Number is "<<num<<"\n";
            label2:
            cout<<"\nDo you want to replay the game [Y/N] :";
            label1:
            cin>>ch;
            if(ch=='Y' || ch=='y')
                guessnum();
            else if(ch=='N' || ch=='n')
                cout<<"\nThank You !\n";
            else 
            {
                cout<<"Please enter valid choice :";
                goto label1;
            }    
        }
        else
        {   if(choice==1 && attempt>=2)
            {
                cout<<"\nHint : ";
                hint(num,attempt,1);
            }
            else if(attempt>=3)
            {
                cout<<"\nHint : ";
                hint(num,attempt,choice);
            }
            goto label;
        }
    }
}

void solution :: hint(int num,int flag,int choice)
{   static bool flg=0;
    switch (flag)
    {
        case 2 :
        if(choice==1)
        {
            if(num%2==0)
                cout<<"The number is Even";
            else 
                cout<<"The number is Odd";
        }
        break;

        case 3:
        if(choice==1)
        {
            if(num>=5)
                cout<<"The number is greater than or equal to 5";
            else 
                cout<<"The number is less than 5";
            break;
        }

        int hcf,cnt;
        hcf=1;
        cnt=0;
        if(num%2==0 && ++cnt<3)
            hcf=hcf*2;
        if(num%3==0 && ++cnt<3)
            hcf=hcf*3;
        if(num%5==0 && ++cnt<3)
            hcf=hcf*5;
        if(num%7==0 && ++cnt<3)
            hcf=hcf*7;
        
        if(cnt>0)
            cout<<"The number is divisible by "<<hcf;
        else
        {
            cout<<"The Unit digit is "<<num%10<<" !";
            flg=1;
        }
        break;

        case 4:
        if(flg)
        {
            cout<<"The difference between Unit and Tens digits is "<<abs(num%10-(num/10)%10);
            flg=0;
            break;
        }
        cout<<"The Tens digit is "<<(num/10)%10;
        break;

        case 5:
        cout<<"The Sum of digits is "<<num%10+(num/10)%10+(num/100);
        break;
    }
}
    


int main()
{   cout<<"\nWELCOME TO NUMBER GUESSING GAME\n\n";
    solution obj;
    obj.guessnum();
}