import java.io.*;
import java.util.*;
class Banker
{
 int n,m;
 int safe[]=new int[n+10];
 boolean safety(int available[],int allocation[][],int need[][],int n1,int m1)
 {
    int n=n1;
    int m=m1;
    int need1[][]=new int[n][m];
    int avail1[]=new int[m];
    int alloc1[][]=new int[n][m];
    
    for(int i=0;i<m;i++)
    avail1[i]=available[i];

    for(int i=0;i<n;i++)
    for(int j=0;j<m;j++)
    alloc1[i][j]=allocation[i][j];

    for(int i=0;i<n;i++)
    for(int j=0;j<m;j++)
    need1[i][j]=need[i][j];

    boolean checksafe[]=new boolean[n];

    for(int i=0;i<n;i++)
    checksafe[i]=false;

    int check=0;
    int check1=0;
    int num=0;
    do
    {
        for(int i=0;i<n;i++)
        {
        boolean flag=true;
        if(checksafe[i]==false)
        {
         for(int j=0;j<m;j++)
        {
        if(avail1[j]<need1[i][j])
         flag=false;
        }
        if(flag==true)
        {
         for(int j=0;j<m;j++)
        avail1[j]=avail1[j]+alloc1[i][j];
        safe[check]=i;
        check++;
        checksafe[i]=true;
        }
        }   
        }
        check1++;
  }while(check<n&&check1<n);


    if(check>n)
    return false;
    else 
    return true;
    
    

 }

boolean reqFunc(int available[],int allocation[][],int need[][],int req[],int procno,int n1,int m1)
{
 int n=n1;
 int m=m1;
 int avail2[]=new int[m];
int alloc2[][]=new int[n][m];
int need2[][]=new int[n][m];
int req1[]=new int[m];
int np=procno;

    for(int i=0;i<m;i++)
    req1[i]=req[i];

    for(int i=0;i<m;i++)
    avail2[i]=available[i];
    
    for(int i=0;i<n;i++)
    for(int j=0;j<m;j++)
    alloc2[i][j]=allocation[i][j];

    for(int i=0;i<n;i++)
    for(int j=0;j<m;j++)
    need2[i][j]=need[i][j];

    boolean flag=true;
    for(int i=0;i<m;i++)
    {
    if(need2[np][i]<=req1[i])
    flag=false;
    }

    if(flag==true)
    {
    for(int i=0;i<m;i++)
    if(avail2[i]<=req1[i])
    flag=false;
    
    if(flag==true)
    {
    for(int i=0;i<m;i++)
    {
    alloc2[np][i]=alloc2[np][i]+req1[i];
    need2[np][i]=need2[np][i]+req1[i];
    avail2[i]=avail2[i]+req1[i];
    }
    
    if(safety(avail2,alloc2,need2,n,m))
    return true;
    else
    System.out.println("Process cannot be granted as it can lead to deadlock");
    }
    else
    System.out.println("Process "+np+" should wait for resources to become available");
    }
    else
    System.out.println("Process has exceeded maximum claim. It cannot be granted");
    return false;

    
}

public void main()throws IOException
{
    InputStreamReader read=new InputStreamReader(System.in);
    BufferedReader obj=new BufferedReader(read);
    
    System.out.println("Enter no. of processes");
    n=Integer.parseInt(obj.readLine());
    System.out.println("Enter no. of resources:");
    m=Integer.parseInt(obj.readLine());

    int available[]=new int[m];
    for(int i=0;i<m;i++)
    {
     System.out.println("Enter no. of available instances of resource "+i);
     available[i]=Integer.parseInt(obj.readLine());
    
    }
    
    System.out.println("Enter allocation matrix:");
    int allocation[][]=new int[n][m];
    for(int i=0;i<n;i++)
    for(int j=0;j<m;j++)
    {
System.out.println("Enter allocated instances of resource "+j+" for process "+i); 
    allocation[i][j]=Integer.parseInt(obj.readLine());
    
    }

System.out.println("Enter maximum matrix:");
    int max[][]=new int[n][m];
    for(int i=0;i<n;i++)
    for(int j=0;j<m;j++)
    {
System.out.println("Enter maximum instances of resource "+j+" for process "+i);    
    max[i][j]=Integer.parseInt(obj.readLine());
    
    }


    int need[][]=new int[n][m];
    for(int i=0;i<n;i++)
    for(int j=0;j<m;j++)
    {
    
    need[i][j]=max[i][j]-allocation[i][j];
    
    }
    System.out.println("Need matrix is: ");
    for(int i=0;i<n;i++)
    {
        for (int j=0;j<m;j++)
        {
            System.out.print(need[i][j]+"  ");
        }
    System.out.println();
}
    
    if(safety(available,allocation,need,n,m))
    {
    System.out.println("System is in Safe State");
    System.out.println("The safe sequence of processes would be:");
    for(int i=0;i<n;i++)
    System.out.println(safe[i]+" ");    
    }

    else
    System.out.println("System is in unsafe state");

    System.out.println("Do you want to place a new request for any process?");
    String ans=obj.readLine();
    if (ans.equalsIgnoreCase("yes"))
    {
    System.out.println("Then enter the process number and the list of requested resources");
    int procno=Integer.parseInt(obj.readLine());
    int req[]=new int[m];
    for(int i=0;i<m;i++)
    req[i]=Integer.parseInt(obj.readLine());
    if(reqFunc(available,allocation,need,req,procno,n,m))
    {   
    System.out.println("Thus, request can be granted");
    for(int i=0;i<m;i++)
    {
    allocation[procno][i]=allocation[procno][i]+req[i];
    need[procno][i]=need[procno][i]+req[i];
    available[i]=available[i]+req[i];
    }
    if(safety(available,allocation,need,n,m))
    {
    System.out.println("System is in Safe State");
    System.out.println("System's Safe sequence:");
    for(int i=0;i<n;i++)
    System.out.println(safe[i]+" ");
    }
    else
    System.out.println("System is in unSafe State");
    }
}
else
System.out.println("Thank You");
}
}
