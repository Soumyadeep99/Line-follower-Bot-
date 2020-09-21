# Line-follower-Bot-
Arduino code for a simple line follower bot

/* FUNCTIONING:
 /*This is the code for a simplest line followr which follows a black line on white surface using two IR sensors. */
 /*Adjust the threshold of the IR sensors for good working using the potentiometer on the module.*/

 /* define L298N or L293D motor control pins */

int rightmotorforward=7;
int rightmotorbackward=4;
int leftmotorforward=5;
int leftmotorbackward=6;

/* define IR pins */

#define leftir 2
#define rightir 3
int leftirreading;
int rightirreading;

void setup() {
  // put your setup code here, to run once:
  pinMode(rightmotorforward,OUTPUT);
  pinMode(rightmotorbackward,OUTPUT);
  pinMode(leftmotorforward,OUTPUT);
  pinMode(leftmotorbackward,OUTPUT);

  pinMode(leftirreading,INPUT);
  pinMode(rightirreading,INPUT);

}

void loop() {
  // put your main code here, to run repeatedly:
  check();
  while(leftirreading==0 && rightirreading==0){      /*both IR sensors is on white surface*/
    forward();
    check();
  }

  while(leftirreading==1 && rightirreading==0){
    check();
    while(leftirreading==1){                         /*left IR sensor is on black surface and right IR sensor is on white surface*/
      left();                                        /*bot turns left*/
      check();
    }    
  }

  while(leftirreading==0 && rightirreading==1){
    check();                                         /*left iR sensor is on white surface and black IR sensor is on black surface*/
    while(rightirreading==1){                        /*bot turns right*/
      right();
      check();
    }
  }

  while(leftirreading==1 && rightirreading==1){      /*bot the sensors are on black surface*/
    stops();                                         /*bot stops moving*/
  }
  

}

void check(){
  leftirreading=digitalRead(leftir);
  rightirreading=digitalRead(rightir);
}

void forward(){
  digitalWrite(rightmotorforward,HIGH);
  digitalWrite(rightmotorbackward,LOW);
  digitalWrite(leftmotorforward,HIGH);
  digitalWrite(leftmotorbackward,LOW);
}

void stops(){
  digitalWrite(rightmotorforward,LOW);
  digitalWrite(rightmotorbackward,LOW);
  digitalWrite(leftmotorforward,LOW);
  digitalWrite(leftmotorbavkward,LOW);
}

void left(){
  digitalWrite(rightmotorforward,HIGH);
  digitalWrite(rightmotorbackward,LOW);
  digitalWrite(leftmotorforward,LOW);
  digitalWrite(leftmotorbackward,LOW);
}

void right(){
  digitalWrite(rightmotorforward,LOW);
  digitalWrite(rightmotorbackward,LOW);
  digitalWrite(leftmotorforward,HIGH);
  digitalWrite(leftmotorbackward,LOW);
}
