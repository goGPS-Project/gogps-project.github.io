# goGPS 1.0
## A MATLAB software for the processing of static GNSS receivers

**goGPS** is an application created for easily processing GNSS raw data. It was originally written specifically to work with GPS single-frequency low-cost receivers but now it can fully exploit **multi-constellation**, **multi-frequency**, **multi-tracking** observations. goGPS can compute Precise Point Positioning (PPP) and network adjustments (NET). It implements two Least Squares (LS) undifferenced engines that we call U1 and U2. U1 is working on combinations of observables (e.g. iono-free observations), it is optimized for computing only PPP solutions. The second engine uses all the frequencies and trackings logged without performing any combinations (ionospheric delays are parameters of the normal equations). U2, the undifferenced - uncombined engine, can compute PPP solutions but also adjustment of baselines or networks with multiple receivers.


The current version of **goGPS** is brought to you by **[GReD srl](https://g-red.eu)**. We have close relations with the world of research and universities and when the development of version 1.0 of goGPS started, we decided to build a piece of software that could help students and researchers in easily manage post-processing tasks of static GNSS receivers. At [GReD](https://g-red.eu) we develop a parallel more complete analysis suite, that uses modified, faster and improved versions of the goGPS engines. Sharing part of our code to the open community allows us to maintain the software and, with your help, receive useful suggestions and bug reports.


In our company we exploit our knowledge in the field of geodesy to provide near-real-time **monitoring services** of land and structures, we are also involved in many **research projects** regarding atmospheric studies with GNSS signals, if you are interested in what we do, or want to cooperate with us, visit our website [https://g-red.eu](https://g-red.eu).

![goGPS_history](/wiki/images/goGPS_MainWindow.png?raw=true)

**Note:** At the moment the software is focused on the processing of permanent stations (geodetic or low-cost) it does not include the possibility to analyze moving receivers.

## A bit of history
**a not very useful tale by Andrea Gatti**

**Disclaimer**: _I'm writing the story as I remember it, I joined the development later and so I may skip many important points but I hope someone will correct and/or expand my narration in the future (this is a wiki)._

It was the year 2007 at Politecnico di Milano - Como campus, in the beginning, there was a set of routines used to teach the students GPS data processing and Kalman filtering. At that time Mirko Reguzzoni and Eugenio Realini started to believe that there could have been space for a new GPS processing software based on MATLAB language. With the help of some students (the founding fathers), they started working on the creation of a complete software exploiting the Kalman filter approach (KF). The first release of goGPS dates back to 2009 when the code was released under GPL and uploaded on the SourceForge platform. The software was used at the university and started to receive contributions from different parts of the World. In the meantime, Eugenio after defending his Ph.D. thesis on goGPS moved to Japan for a post-doc focused on tropospheric studies, there he wrote most of the code necessary for computing PPP solutions. In 2014 Eugenio came back to Italy to join some of his ex-colleagues at Politecnico in a spin-off called GReD and released a first-version able to perform precise point positioning with a Kalman Filter approach.

![goGPS_history](/wiki/images/goGPS_History.png?raw=true)

When I started working on goGPS at the end of 2016, the software used a fixed data structure that limited the usage of a maximum of two frequencies and was not ready for the modern constellations. It was very complicated to expand it or modify the processing sequence; what started as a performance improvement soon became a total rewrite of the software. I decided to use object-oriented programming for greater flexibility and modularity but the Kalman filter approach was still a limiting performance factor (MATLAB is notoriously slower in performing iterative processing). Eugenio made some preliminary tests on a batch least-square (LS) engine using differenced and combined observations that were very promising, so when Giulio Tagliaferro joined the team of GReD and we had to develop a new engine he proposed and wrote a first undifferenced batch processor that is still the core behind goGPS pre-processing. MATLAB excels in working with matrixes and the new engine is much faster than the old one with KF, but we were always working with post-processing of quasi-static (permanent) stations, and we optimized the processing for them, now we no longer support moving receivers. In 2018 I left Politecnico and joined [GReD srl](https://g-red.eu); since then 99% of the current development is supported by our spin-off. In 2019, Giulio and I took care of most of the development, improving the software that now has very few lines of code in common with the original version. The last release with KF is 0.5.x and can be found now in the legacy branch. 

The new 1.0 release is a completely new software with two engines (combined and uncombined) able to perform PPP and Network adjustments that can be easily expanded to support other approaches and be the base for testing new ideas. We expected to release goGPS 1.0 in 2020 but we were continuously adding new features and options that made the software very unstable. We decided in 2021 to feature freeze goGPS, we removed some features present in the beta version and we cleaned a bit the code (work in progress). At the moment we are working on the version 1.0 Release Candidate. Maintaining the development of the internal software of GReD and porting some cutting edge algorithms (in a simplified way) in goGPS required too much effort, but we are committed to produce our first stable release of our open-source software. When ready, we will also provide compiled executable of goGPS.

## About
### List of the contributors of the original goGPS
Developers of the original core:

* Mirko Reguzzoni
* Eugenio Realini

Founding fathers:

* Alessio Dominioni
* Roberto Teruzzi
* Marco Colla
* Marcello Palmulli
* Michele Tettamanti

Contributors (in temporal order):

* Sara Lucca
* Lisa Pertusini
* Daniele Sampietro
* Luana Valentini
* Alemu Befkadu Nigussie
* Ivan Reguzzoni
* Stefano Caldera
* Damiano Triglione
* Giuliano Sironi
* Antonio Herrera Olmo
* Andrea Gatti
* Hendy F. Suhandri
* Andrea Nardo
* Daisuke Yoshida
* Marco Negretti
* Yuya Iwaki
* Louis Osei-Poku
* Paiyuan Zhou

### List of the contributors of the Java version of goGPS
Developers of the original goGPS Java core:

* Eugenio Realini
* Lorenzo Patocchi

Other Contributors:

* Daisuke Yoshida
* Emanuele Ziglioli

### List of the contributors of the new goGPS 1.0
Developers of the new goGPS core:

* Andrea Gatti
* Giulio Tagliaferro

Other Contributors:

* Eugenio Realini
* Stefano Caldera
* Lisa Pertusini
* Andreas Krietemeyer
* Alice Bonfiglio
* Stefano Barindelli
* Alessandra Mascitelli
* Cristina Borghi
