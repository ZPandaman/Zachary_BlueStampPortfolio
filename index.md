# Automatic Arduino Pet Feeder
This project involves the development of an automated pet feeding system. Utilizing an Arduino microcontroller and a servo motor, the device dispenses pre-measured portions of food at scheduled intervals. The goal is to create a reliable, user-friendly solution that ensures consistent feeding times, supports pet health, and offers convenience for pet owners.

<!--- You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions: -->
<!--- ```HTML  -->
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
![Headstone Image](photo.svg)

| **Engineer** | **School** | **Area of Interest** | **Grade** |
| Zachary S | Saratoga High School | Electrical Engineering | Incoming Senior|

# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=Er9wOi8vWgo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

I have completed my pre-modifications auto pet feeder. For this milestone, my goal was simple: Assembly. This took a while. Originally, I had to wait a couple of days for the resources to arrive, then I got straight to work. Slowly, I attached pieces of wood until I had a completed body that could hold my bottle, Arduino, and servo (on a 3D-printed board). I faced a few challenges; however, they're important. Originally, I had the servo connected inversely, and I drilled too quickly, partially splitting wood.

# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/8yCQyTd6C6A?si=BSUMwjBY3fnUcX5J" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

I have successfully uploaded a pre-written program to my Arduino, which controls the servo motor as intended. So far, the development process has been more straightforward than I initially expected; I anticipated greater difficulty in assembling the components and getting the system operational. My servo motor connects to my Arduino in 3 locations: PWM Port #9, 5V, and Ground. PWM Port #9 sends code from the Arduino to the servo motor. 5V powers the whole machine, and Ground prevents the machine from overloading. One minor challenge I encountered was navigating the Arduino IDE, particularly in locating the Verify (Check) and Upload functions, which were not immediately intuitive. As I prepare for the next project milestone, my focus will be on constructing the physical housing for the feeder, which will complete the first functional prototype.

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/Kml1ljBbxgg?si=0Z-JtJnZRnDKTnNK" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My project utilizes an Arduino microcontroller in conjunction with a servo motor to create an automated mechanism that dispenses food after a predetermined interval. I have developed two schematics: one outlining the overall design and functional plan for the feeder, and another detailing the wiring connections between the Arduino and the servo. The hardware components have been correctly assembled, with the servo successfully interfaced with the Arduino. The next phase of the project involves programming the motor to execute the timed feeding function, followed by constructing the physical enclosure for the feeder system.

# Schematics 
![Headstone Image](Bodydesign.svg)
![Headstone Image](electronicsArduino.svg)

# Code

```c++
#include <Servo.h>

#define FEED_INTERVAL   1   // minutes between feeding time

const byte servoPin = 9;      // pin used to command the servo motor
const int waitingTime = FEED_INTERVAL;

Servo servo;

volatile unsigned long sec;
const unsigned long feedInterval = (unsigned long) FEED_INTERVAL * (unsigned long) 5;  // expressed in seconds

/**
   Stop the food from flowing
*/
void feederClose() {
  servo.write(90);
  delay(175);
  servo.write(0);
}

/**
   release a ration of food
*/
void feederOpen() {
  servo.write(0);
  delay(175);
  servo.write(90);
}

// Interrupt is called once a millisecond,
SIGNAL(TIMER0_COMPA_vect)
{
  if (millis() % 1000 == 0) { // if a second has passed
    sec++;  // increment the seconds counter
    Serial.print("Second: ");
    Serial.print(sec);
    Serial.print(" of ");
    Serial.println(feedInterval);
  }
}

void setup() {
  Serial.begin(9600);
  OCR0A = 0xAF; // set the timer interrupt
  TIMSK0 |= _BV(OCIE0A);
  servo.attach(servoPin);
  Serial.println("System initialized");
}

void loop() {
  Serial.println("Waiting...");
  sec = 0;  // reset the counter
  while (feedInterval > sec);   // wait until the time interval is elapsed
  Serial.println("Feeding the pet :)");
  feederOpen();
  delay(300);
  feederClose();
}
```

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| ELEGOO UNO R3 Board | Knowledge System | $13.99 | <a href="https://www.amazon.com/ELEGOO-Board-ATmega328P-ATMEGA16U2-Compliant/dp/B01EWOE0UU/ref=asc_df_B01EWOE0UU?mcid=3c20e862567d3232bda82cbee4dcb2bc&hvocijid=14813818644327468140-B01EWOE0UU-&hvexpln=73&tag=hyprod-20&linkCode=df0&hvadid=721245378154&hvpos=&hvnetw=g&hvrand=14813818644327468140&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9032183&hvtargid=pla-2281435179498&psc=1"> Link </a> |
| Micro Servo Motor | Motor | $3.95 | <a href="https://www.pishop.us/product/sg90-180-degrees-9g-micro-servo-motor-tower-pro/?srsltid=AfmBOopBU3qoo6JMPLqnKSsB_kBYvWlNnjVJRjSDOiPPOsoE6C3P-Kll"> Link </a> |
| Breadboard Jumper Wires | Connecting Arduino to Servo | $7.49 | <a href="https://www.amazon.com/EDGELEC-Breadboard-Multicolored-1pin-1pin-Connector/dp/B07GD431C1?source=ps-sl-shoppingads-lpcontext&ref_=fplfs&smid=A3S1JN3RP90N86&gQT=1&th=1"> Link </a> |
| Cardboard | Food dispensing lid & Servo platform | $34.20 | <a href="https://www.amazon.com/BOX-USA-BHD888DW-Double-Boxes/dp/B01D2745AS?source=ps-sl-shoppingads-lpcontext&ref_=fplfs&smid=ATVPDKIKX0DER&gQT=1&th=1"> Link </a> |
| Bottle | Food Storage | $14.99 | <a href="https://www.amazon.com/Plastic-Bottles-Evident-MT-Products/dp/B0DV9KJNQ8/ref=sr_1_56?crid=3IE1TYC64QSNV&dib=eyJ2IjoiMSJ9.Z2YPjEa4clS-jR9nWaJfQh1PcLc-r0epd42-jKv6AbFuHgM9oFKa_gfJEFG6ExS2IUDIq56qOaNplgxsXvxG6X2PREHEsYrltsGPL137eyB87JjW1ChnRAFvkEOJCtl3VOW7mEf0PcYo330qOX3gFtEvZTQ6161G9XN_glbNQIXr7ixrhTIhkMTXw4fJSH8PawYrWRGr-61ZXep0IZQ7gFw5rI26Dhb573TSoe004UABUIEZ2WLYb0gaV26LDLwCmxgSJ2T5E_n2juRmcZhj45KPvig9FtuuiSXQmEjUlIw.et3zReqWTUa92pLCzfwuu976dLGY44iiEEx4fbXSutQ&dib_tag=se&keywords=gallon%2Bbottle&qid=1752010065&sprefix=gallonbottle%2Caps%2C130&sr=8-56&th=1"> Link </a> |
| USB Cable | Powering device | $6.39 | <a href="https://www.amazon.com/Amazon-Basics-External-Gold-Plated-Connectors/dp/B00NH13DV2/ref=asc_df_B00NH13DV2?mcid=7b0ef2f745a03442894b878cac036b6e&hvocijid=17925455797167343296-B00NH13DV2-&hvexpln=73&tag=hyprod-20&linkCode=df0&hvadid=721245378154&hvpos=&hvnetw=g&hvrand=17925455797167343296&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9032183&hvtargid=pla-2281435179778&th=1"> Link </a> |
| Wood | Holds Food Storage | $7.64 | <a href="https://www.amazon.com/dp/B0D73D2RQT?ref=fed_asin_title&th=1"> Link </a> |
| Wood Boards | Holds Arduino | $7.99 | <a href="https://www.amazon.com/Pack-Basswood-Sheets-Crafts-Architectural/dp/B0CZDBZ6WQ/ref=sxin_15_pa_sp_search_thematic_sspa?content-id=amzn1.sym.2da95b6c-f59a-4699-bc43-d0ff036c6388%3Aamzn1.sym.2da95b6c-f59a-4699-bc43-d0ff036c6388&crid=3HO8820AZ1VFR&cv_ct_cx=flat%2Bwood&keywords=flat%2Bwood&pd_rd_i=B0DGPYSXTP&pd_rd_r=488bedc4-b63b-4788-a78a-373ff9f5d17e&pd_rd_w=EhHIS&pd_rd_wg=eiBXP&pf_rd_p=2da95b6c-f59a-4699-bc43-d0ff036c6388&pf_rd_r=W4MCFDZVWVR5D0YF11ZN&qid=1752617818&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=flat%2Bwo%2Caps%2C163&sr=1-4-6024b2a3-78e4-4fed-8fed-e1613be3bcce-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&th=1"> Link </a> | 
| Screws | Holds wood together | $9.99 | <a href="https://www.amazon.com/HanTof-Phillips-Self-Tapping-Recessed-Assortment/dp/B09CPHWX7P/ref=sr_1_9?crid=2XDJGVXZUSW8A&dib=eyJ2IjoiMSJ9.hFNKFLF0EAmvspwRiW_CJqKzN3qg3tZ90VLFsSRKfUWzBcRw8Nvhe-Xjqdh-cINwffssnx06WIBjAX9Iwea0QCNhy7il5frfWjtl7koJS6ZM1f9HRNS4enyhRQfdimqhusC3vplPssYaF9Jz76lrk9_nM1ENhSdLUvSxo2V8EYn4KtI3cDSFQphh3i13Y36JDAsNGvXbapvhIplh70-vRxjedfWWV3sCrG6TxJZJ-QQ.xhse6FbYE0MQyP9AjfpBazhEgx1Kar6zM342jK9C3IA&dib_tag=se&keywords=wood%2Bscrews%2Bmillimeters&qid=1752618089&sprefix=wood%2Bscrews%2Bmillimeter%2Caps%2C138&sr=8-9&th=1"> Link </a> |

# Tools

| **Tool** | **Price** | **Link** |
| Box Cutter | $10.39 | <a href="https://www.amazon.com/Retractable-Cardboard-Package-Scraper-Packages/dp/B07YMPQJK7/ref=asc_df_B07YMPQJK7?mcid=7bde0af999933ca19ad4bed2c8c70d5a&hvocijid=4749166825711708819-B07YMPQJK7-&hvexpln=73&tag=hyprod-20&linkCode=df0&hvadid=721245378154&hvpos=&hvnetw=g&hvrand=4749166825711708819&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9032183&hvtargid=pla-2281435179098&th=1"> Link </a> |
| Ruler | $2.99 | <a href="https://www.amazon.com/Plastic-Rulers-School-Assorted-Colors/dp/B0C1YXFP99/ref=asc_df_B0C1YXFP99?mcid=c61bb3da5a783aa0adfb36d028c553ff&hvocijid=16395250037265102296-B0C1YXFP99-&hvexpln=73&tag=hyprod-20&linkCode=df0&hvadid=721245378154&hvpos=&hvnetw=g&hvrand=16395250037265102296&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9032183&hvtargid=pla-2281435178858&th=1"> Link </a> |
| Drill |  $107.00 | <a href="https://www.amazon.com/BOSCH-PS31-2A-Two-Speed-Driver-Batteries/dp/B003BEE2LU/ref=sr_1_1_sspa?crid=1UPOPBAV6EAF7&dib=eyJ2IjoiMSJ9.gxAfO5xViGBxrkc8dVPEQOLcmInO0Or7UEOx4B6eJ-MRgZkgvkC0ofuPIu93yR4As1DDElfiKFqGJX17g77DxDbOsY27F-SLK0onu80MJPRnM3M4KN6vzhgcKtAxaCojFBU4M2soK7g88tx5x0_3Q-Sn4TUc72ByRpkg59ZWObFRXx0Cxw2W41NPzwrVhlWpVK3FVVoG26D7SmVBojCq9JQZYh0GIJpYx495OPgBgPtJBbfM1iboiTkhuz4S2eztoWBtQ_ZbsX4b6yOCbQXh3TX-SelUFPYCCjMnGJ0sfas.WREaN4gtlRF3R9ajeet0XXxNW2IbLyu5xnxhiUzlylg&dib_tag=se&keywords=drill&qid=1752617963&s=home-garden&sprefix=dril%2Cgarden%2C185&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| Hand Saw | $22.95 | <a href="https://www.amazon.com/DNA-MOTORING-Bi-Ground-Universal-TOOLS-00589/dp/B0DHKDCYTG/ref=sr_1_6?crid=GTGV8WS4ZY2K&dib=eyJ2IjoiMSJ9.bMZcXjVWu1gfeR4v_ZbRwdcoQSHpAESJIrUaoPtte-OLnCJyNDesad2zSlBrUVbGWsYbMhFN8-bojc9JLrlXydGj48xkXfYYPMzX3MdQmdKP1jOgTLoF6q2OQwmA5BvGClYdM4M313PEnASF1I7eck4GjUu7xg0u05zFfavPFhCafb6KgNe-PxtmsBUIhsKwGKhEUizr7S5Wxyxx0TqOWY0wY3ldI3KlEF6Ul242mA_rEkXd7XFuRiZVBZJKJCPlo6pFS2ej6dUrf7MmXLJUlBwPrYV9T0fakM6shRQtGzI.WkjYPGdPrUp2OAHQs2nP30anJzPubhxIGJB2i8Jlr-A&dib_tag=se&keywords=handsaw&qid=1752618006&s=home-garden&sprefix=handsaw%2Cgarden%2C150&sr=1-6&th=1"> Link </a> |

# Additional Resources

A 3D printer is needed to print the holder of the servo that has a hole for the water bottle.

