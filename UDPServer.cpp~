#include <string.h>
#include <fstream>
#include <iostream>
#include <sstream>
#include <unistd.h>
#include <arpa/inet.h>
#include <netinet/in.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <stdlib.h>
#include <math.h>
#include <pthread.h> 
#include "gpio.h"

#define BUFLEN 512
#define SRV_IP "192.168.7.2"
using namespace std;

int main()
{
	    gpio gpio72("72"),gpio69("69");
	    gpio72.mode("72","in");
		
	    gpio69.mode("69","in");
	    int count=0;
	    struct sockaddr_in my_addr, cli_addr;
	    int sockfd, i; 
	    socklen_t slen=sizeof(cli_addr);
	    char buf[BUFLEN];	
            char val[BUFLEN];
	    char c;
	    if ((sockfd = socket(AF_INET, SOCK_DGRAM, IPPROTO_UDP))==-1)
	      cout << "socket" << endl;
	    else
	      printf("Server : Socket() successful\n");

	    bzero(&my_addr, sizeof(my_addr));
	    my_addr.sin_family = AF_INET;
	    my_addr.sin_port = htons(45678);
	    my_addr.sin_addr.s_addr = htonl(INADDR_ANY);

	    if (bind(sockfd, (struct sockaddr* ) &my_addr, sizeof(my_addr))==-1)
	      cout << "bind" << endl;
	    else
	      printf("Server : bind() successful\n");

	    
	   

	    while(1)
	    {
		if (recvfrom(sockfd, buf, BUFLEN, 0, (struct sockaddr*)&cli_addr, &slen/*sizeof(cli_addr)*/)==-1)
		    cout << "recvfrom()" << endl;;
		printf("Received packet from %s:%d\nData: %s\n\n",
		       inet_ntoa(cli_addr.sin_addr), ntohs(cli_addr.sin_port), buf);
	
		
		
			while(1)
			{
			
				FILE * fs;
				string path="/sys/devices/ocp.3/helper.12/AIN0";  
				char * Fpath;
				int move;
				Fpath=&path[0];
				fs=fopen(Fpath,"r");
				fgets(val,BUFLEN,fs);
				sscanf(val, "%d", &move);
				
				buf[0]=gpio72.read("72").at(0);
				if(move<50){
				buf[1]='3';
				buf[2]='0';
				}
				else if(move>=50 && move<350){
				buf[1]='2';
				buf[2]='0';
				}
				else if(move>=350 && move<890){
				buf[1]='1';
				buf[2]='0';
				}
				else if(move>=890 && move<=900){
				buf[1]='0';
				buf[2]='0';
				}
				else if(move>900 && move<=1450){
				buf[1]='0';
				buf[2]='1';
				}
				else if(move>1450 && move<=1750){
				buf[1]='0';
				buf[2]='2';
				}
				else {
				buf[1]='0';
				buf[2]='3';
				}
				
				c=gpio69.read("69").at(0);
				if(c=='0'&& count<10){
				  buf[3]=c;
				  count++;
				  
				}
				else if(c=='1'){
				  buf[3]=c;
				  count=0;
				}
				else{
				  buf[3]='1';
				  count++;
				  if(count==500)
				  count=0;
				}
				buf[4] = '\0';
				//cout<<"buf: "<<buf<<"count: "<<count<<" move:"<<move<<endl;
				if(sendto(sockfd, buf, strlen(buf), 0, (struct sockaddr*)&cli_addr, sizeof(cli_addr))==-1)
					perror("sendto error");
				fclose(fs);
				
			}
		
		
	 	}
	    close(sockfd);
		return 0;
	}




