# LTE-WiFi-Coexistence
This repository includes the various codes to implement the various methods that have been proposed in the PhD thesis of Moawiah Alhulayil under title 'Spectrum Coexistence Mechanisms for Mobile Networks in Unlicensed Frequency Bands', University of Liverpool, UK, June, 2021.



%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%  1. Cat 4 LBT Method (3GPP Standard):  %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

   - The LAA eNB starts a backoff process by picking a random number N which is unifomly distributed. 
   - N is between [0, q-1] where q-1 is the upper bound of the CW size. 
   - The upper bound of the CW, q-1, varies between {15, 31, 63}.
   - The upper bound of the CW, q-1, is initialised with 15 and it is exponentially increased based on the 80% of the HARQ ACK feedbacks.


                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%% Programming %%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

   * To generate the simulation results for the previous method for different traffic loads FTP Lambda ={0.5,1.5,2.5,3.0} :

   - Use the @@@lbt-access-manager((original)).cc@@@ file within 'Model' file.
   - Change the FTP Lambda as 0.5,1.5,2.5,3.0 from the file 'laa-wifi-indoor.cc'.
   - Choose the number of STAs/UEs per cell from the file 'laa-wifi-indoor.cc'.
   - Dectivate the following lines in the code of file 'dca-txop.cc'. The lines are:

       .AddAttribute ("CwMin", "Sets the minimum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMinCw),
                  MakeUintegerChecker<uint32_t> ())

      .AddAttribute ("CwMax", "Sets the maximum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMaxCw),
                  MakeUintegerChecker<uint32_t> ())



   - Dectivate the following lines in the code of file 'laa-wifi-indoor.cc'. The lines are: 
          
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMin", UintegerValue ());
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMax", UintegerValue ());

    NOTE: We added the previous lines to the 'dca-txop.cc' and 'laa-wifi-indoor.cc' files 


    - Make sure that the following lines in the 'lbt-access-manager((original)).cc' file are as follows:

    .AddAttribute ("MinCw", "The minimum value of the contention window.",
                   UintegerValue (15),
                   MakeUintegerAccessor (&LbtAccessManager::m_cwMin),
                   MakeUintegerChecker<uint32_t> ())
    .AddAttribute ("MaxCw", "The maximum value of the contention window. For the priority class 3 this value is set to 63, and for priority  
                    class 4 it is 1023.",
                   UintegerValue (63),
                   MakeUintegerAccessor (&LbtAccessManager::m_cwMax),
                   MakeUintegerChecker<uint32_t> ()) 


    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'run-laa-wifi-indoor-ftp.sh'.
    - Change the base_duration=120 based on the FTP Lmabda as follwing to get a total of 240 sec:


       For FTP Lambda= 0.5, the base_duration=120.
       For FTP Lambda= 1.5, the base_duration=360.
       For FTP Lambda= 2.5, the base_duration=600.
       For FTP Lambda= 3.0, the base_duration=720.

    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'plot-laa-wifi-indoor-ftp.sh'.











%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%  2. Variable CW Methods (WCNC Paper):  %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


  * Method A:
   - The LAA eNB starts a backoff process by picking a random number N which is unifomly distributed. 
   - N is between [0, q-1] where q-1 is the upper bound of the CW size. 
   - The upper bound of the CW, q-1, varies between {50%, 95%, 100%} points of the CDF of the ON times of the existing Wi-Fi.
   - The upper bound of the CW, q-1, is initialised with 50% criterion of the CDF of the ON times of the Wi-Fi and it is increased based on the
     80% of the HARQ ACK feedbacks (e.g., for lambda=0.5, the upper bound of the LAA CW size varies between {6, 19, 23}).

 * Method B:
   - The LAA eNB starts a backoff process by picking a random number N which is unifomly distributed. 
   - N is between [0, q-1] where q-1 is the upper bound of the CW size. 
   - The upper bound of the CW, q-1, varies between {50%, 100%} points of the CDF of the ON times of the existing Wi-Fi.
   - The upper bound of the CW, q-1, is initialised with 50% criterion of the CDF of the ON times of the Wi-Fi and it is increased based on the
     80% of the HARQ ACK feedbacks (e.g., for lambda=0.5, the upper bound of the LAA CW size varies between {6, 23}).

               

                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%% Programming %%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

   * To generate the simulation results for the previous method for different traffic loads FTP Lambda ={0.5,1.5,2.5,3.0} :

   - Use the @@@lbt-access-manager((variable methods)).cc@@@ file within 'Model' file.
   - Change the FTP Lambda as 0.5,1.5,2.5,3.0 from the file 'laa-wifi-indoor.cc'.
   - Choose the number of STAs/UEs per cell from the file 'laa-wifi-indoor.cc'.
   - Dectivate the following lines in the code of file 'dca-txop.cc'. The lines are:

       .AddAttribute ("CwMin", "Sets the minimum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMinCw),
                  MakeUintegerChecker<uint32_t> ())

      .AddAttribute ("CwMax", "Sets the maximum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMaxCw),
                  MakeUintegerChecker<uint32_t> ())



   - Dectivate the following lines in the code of file 'laa-wifi-indoor.cc'. The lines are: 
          
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMin", UintegerValue ());
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMax", UintegerValue ());

    NOTE: We added the previous lines to the 'dca-txop.cc' and 'laa-wifi-indoor.cc' files 


   - Make sure that the following lines in the 'lbt-access-manager((variable methods)).cc' file are as follows:

    .AddAttribute ("MinCw", "The minimum value of the contention window.",
                   UintegerValue (6),
                   MakeUintegerAccessor (&LbtAccessManager::m_cwMin),
                   MakeUintegerChecker<uint32_t> ())
    .AddAttribute ("MaxCw", "The maximum value of the contention window. For the priority class 3 this value is set to 63, and for priority  
                    class 4 it is 1023.",
                   UintegerValue (23),
                   MakeUintegerAccessor (&LbtAccessManager::m_cwMax),
                   MakeUintegerChecker<uint32_t> ()) 
    

    - update the following lines in the 'lbt-access-manager((variable methods)).cc' as follwos based on the values of 50%, 95% and 100% points:
    

      // m_cw = std::min ( (m_cw.Get () + 1 + 13) - 1, m_cwMax); // This is for my variable method A

      // m_cw = std::min ( m_cwMax, m_cwMax);  // This is for my variable method B

    NOTE: MinCW =6, MaxCW=23, and min ( (m_cw.Get () + 1 + 13) - 1, m_cwMax); these settings for lambda=0.5 since the correspondent values of 
          {50%, 95%, 100%} are {6, 19, 23}. You should change MinCW, MaxCW and the rule of m_cw based on lambda value and based on the method 
          if it is A or B method.

    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'run-laa-wifi-indoor-ftp.sh'.
    - Change the base_duration=120 based on the FTP Lmabda as follwing to get a total of 240 sec:


       For FTP Lambda= 0.5, the base_duration=120.
       For FTP Lambda= 1.5, the base_duration=360.
       For FTP Lambda= 2.5, the base_duration=600.
       For FTP Lambda= 3.0, the base_duration=720.

    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'plot-laa-wifi-indoor-ftp.sh'.









%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%  3. Static CW Method (ready paper):  %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

 

   - The LAA eNB starts a backoff process by picking a random number N which is unifomly distributed. 
   - N is between [0, q-1] where q-1 is the upper bound of the CW size. 
   - The upper bound of the CW, q-1, is static and it is set to be the {100%} point of the CDF of the ON times of the existing Wi-Fi.
   - No adaptation for the upper size of the CW based on the HARQ ACK feedbacks.

                

                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%% Programming %%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


   * To generate the simulation results for the previous method for different traffic loads FTP Lambda ={0.5,1.5,2.5,3.0} :

   - Use the @@@lbt-access-manager((static CW)).cc@@@ file within 'Model' file.
   - Change the FTP Lambda as 0.5,1.5,2.5,3.0 from the file 'laa-wifi-indoor.cc'.
   - Choose the number of STAs/UEs per cell from the file 'laa-wifi-indoor.cc'.
   - Activate the following lines in the code of file 'dca-txop.cc'. The lines are:

       .AddAttribute ("CwMin", "Sets the minimum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMinCw),
                  MakeUintegerChecker<uint32_t> ())

      .AddAttribute ("CwMax", "Sets the maximum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMaxCw),
                  MakeUintegerChecker<uint32_t> ())



   - Activate the following lines in the code of file 'laa-wifi-indoor.cc'. The lines are: 
          
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMin", UintegerValue ());
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMax", UintegerValue ());

    NOTE: We added the previous lines to the 'dca-txop.cc' and 'laa-wifi-indoor.cc' files 


    - Calibrate the following line in the @@@lbt-access-manager((Static CW)).cc@@@ with the suitable values:

         return (m_rng->GetInteger (0,23)); // this is because 23 is the correspondent value of 100% of the wifi On time CDF. 


    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'run-laa-wifi-indoor-ftp.sh'.
    - Change the base_duration=120 based on the FTP Lmabda as follwing to get a total of 240 sec:


       For FTP Lambda= 0.5, the base_duration=120.
       For FTP Lambda= 1.5, the base_duration=360.
       For FTP Lambda= 2.5, the base_duration=600.
       For FTP Lambda= 3.0, the base_duration=720.

    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'plot-laa-wifi-indoor-ftp.sh'.








%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%  4. The fixed N Method (to be submitted within the Journal paper):  %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


   - The LAA eNB does not need a backoff process (i.e., no picking for any random number N).
   - The LAA eNB should wait a fixed time equal to N which is set to be the {100%} point of the CDF of the ON times of the existing Wi-Fi.
   - No adaptation for the upper size of the CW based on the HARQ ACK feedbacks.



                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%% Programming %%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


   * To generate the simulation results for the previous method for different traffic loads FTP Lambda ={0.5,1.5,2.5,3.0} :

   - Use the @@@lbt-access-manager((Fixed N Method)).cc@@@ file within 'Model' file.
   - Change the FTP Lambda as 0.5,1.5,2.5,3.0 from the file 'laa-wifi-indoor.cc'.
   - Choose the number of STAs/UEs per cell from the file 'laa-wifi-indoor.cc'.
   - Activate the following lines in the code of file 'dca-txop.cc'. The lines are:

       .AddAttribute ("CwMin", "Sets the minimum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMinCw),
                  MakeUintegerChecker<uint32_t> ())

      .AddAttribute ("CwMax", "Sets the maximum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMaxCw),
                  MakeUintegerChecker<uint32_t> ())



   - Activate the following lines in the code of file 'laa-wifi-indoor.cc'. The lines are: 
          
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMin", UintegerValue ());
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMax", UintegerValue ());

    NOTE: We added the previous lines to the 'dca-txop.cc' and 'laa-wifi-indoor.cc' files 


    - Calibrate the following line in the @@@lbt-access-manager((Fixed N Method)).cc@@@ with the suitable values:

         return (m_rng->GetInteger (23,23)); // this is because 23 is the correspondent value of 100% of the wifi On time CDF for lambda=0.5. 


    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'run-laa-wifi-indoor-ftp.sh'.
    - Change the base_duration=120 based on the FTP Lmabda as follwing to get a total of 240 sec:


       For FTP Lambda= 0.5, the base_duration=120.
       For FTP Lambda= 1.5, the base_duration=360.
       For FTP Lambda= 2.5, the base_duration=600.
       For FTP Lambda= 3.0, the base_duration=720.

    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'plot-laa-wifi-indoor-ftp.sh'.








%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%% 5. Task 1: Setting the LAA CW based on the ((Mode value)) of the ON times of the existing Wi-Fi network:%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%






%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%  Method 1: N between [Nmode, q-1]: (Modifying Cat 4 LBT Method):  %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
      
   - The LAA eNB starts a backoff process by picking a random number N which is unifomly distributed. 
   - N is between [Nmode, q-1] where q-1 is the upper bound of the CW size and Nmode is the mode value of the CDF of the ON times of the
     existing Wi-Fi.
   - The upper bound of the CW, q-1, varies between {15, 31, 63}.
   - The upper bound of the CW, q-1, is initialised with 15 and it is exponentially increased based on the 80% of the HARQ ACK feedbacks.



                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%% Programming %%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

* To generate the simulation results for the previous method for different traffic loads FTP Lambda ={0.5,1.5,2.5,3.0} :

   - Use the @@@lbt-access-manager((N between Nmode and CWcat4lbt)).cc@@@ file within 'Model' file.
   - Change the FTP Lambda as 0.5,1.5,2.5,3.0 from the file 'laa-wifi-indoor.cc'.
   - Choose the number of STAs/UEs per cell from the file 'laa-wifi-indoor.cc'.
   - Dectivate the following lines in the code of file 'dca-txop.cc'. The lines are:

       .AddAttribute ("CwMin", "Sets the minimum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMinCw),
                  MakeUintegerChecker<uint32_t> ())

      .AddAttribute ("CwMax", "Sets the maximum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMaxCw),
                  MakeUintegerChecker<uint32_t> ())



   - Dectivate the following lines in the code of file 'laa-wifi-indoor.cc'. The lines are: 
          
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMin", UintegerValue ());
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMax", UintegerValue ());

    NOTE: We added the previous lines to the 'dca-txop.cc' and 'laa-wifi-indoor.cc' files 


    - Make sure that the following lines in the 'lbt-access-manager((N between Nomde and CWcat4lbt)).cc' file are as follows:

    .AddAttribute ("MinCw", "The minimum value of the contention window.",
                   UintegerValue (15),
                   MakeUintegerAccessor (&LbtAccessManager::m_cwMin),
                   MakeUintegerChecker<uint32_t> ())
    .AddAttribute ("MaxCw", "The maximum value of the contention window. For the priority class 3 this value is set to 63, and for priority  
                    class 4 it is 1023.",
                   UintegerValue (63),
                   MakeUintegerAccessor (&LbtAccessManager::m_cwMax),
                   MakeUintegerChecker<uint32_t> ()) 



     - Make sure to edit the following lines in the 'lbt-access-manager((N between Nomde and CWcat4lbt)).cc' to select Nmode of each FTP lambda
       as follows:

          uint32_t
          LbtAccessManager::GetBackoffSlots ()
          {
              NS_LOG_FUNCTION (this);
             // Integer between 0 and m_cw
            return (m_rng->GetInteger (11, m_cw.Get ()));

           }


       NOTE: Nmode is 11 for lambda=0.5 for UE/STA=5.


    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'run-laa-wifi-indoor-ftp.sh'.
    - Change the base_duration=120 based on the FTP Lmabda as follwing to get a total of 240 sec:


       For FTP Lambda= 0.5, the base_duration=120.
       For FTP Lambda= 1.5, the base_duration=360.
       For FTP Lambda= 2.5, the base_duration=600.
       For FTP Lambda= 3.0, the base_duration=720.

    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'plot-laa-wifi-indoor-ftp.sh'.







%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%  Method 2: N between [Nmode, q-1]: (Modifying our Static CW Method):  %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

      
   - The LAA eNB starts a backoff process by picking a random number N which is unifomly distributed. 
   - N is between [Nmode, q-1] where q-1 is the upper bound of the CW size and Nmode is the mode value of the CDF of the ON times of the
     existing Wi-Fi. 
   - The upper bound of the CW, q-1, is static and it is set to be the {100%} point of the CDF of the ON times of the existing Wi-Fi.
   - No adaptation for the upper size of the CW based on the HARQ ACK feedbacks.

           

                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%% Programming %%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


   * To generate the simulation results for the previous method for different traffic loads FTP Lambda ={0.5,1.5,2.5,3.0} :

   - Use the @@@lbt-access-manager((N between Nmode and 100%)).cc@@@ file within 'Model' file.
   - Change the FTP Lambda as 0.5,1.5,2.5,3.0 from the file 'laa-wifi-indoor.cc'.
   - Choose the number of STAs/UEs per cell from the file 'laa-wifi-indoor.cc'.
   - Activate the following lines in the code of file 'dca-txop.cc'. The lines are:

       .AddAttribute ("CwMin", "Sets the minimum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMinCw),
                  MakeUintegerChecker<uint32_t> ())

      .AddAttribute ("CwMax", "Sets the maximum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMaxCw),
                  MakeUintegerChecker<uint32_t> ())



   - Activate the following lines in the code of file 'laa-wifi-indoor.cc'. The lines are: 
          
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMin", UintegerValue ());
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMax", UintegerValue ());

    NOTE: We added the previous lines to the 'dca-txop.cc' and 'laa-wifi-indoor.cc' files 


    - Calibrate the following line in the @@@lbt-access-manager((N between Nmode and 100%)).cc@@@ with the suitable values:

          return (m_rng->GetInteger (11,23)); // this is because Nmode=11 and 23 is the correspondent value of 100% of the wifi On time CDF 
                                                 for lambda=0.5. 


    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'run-laa-wifi-indoor-ftp.sh'.
    - Change the base_duration=120 based on the FTP Lmabda as follwing to get a total of 240 sec:


       For FTP Lambda= 0.5, the base_duration=120.
       For FTP Lambda= 1.5, the base_duration=360.
       For FTP Lambda= 2.5, the base_duration=600.
       For FTP Lambda= 3.0, the base_duration=720.

    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'plot-laa-wifi-indoor-ftp.sh'.








%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%  Method 3: N = Nmode: (Modifying our fixed N Method):  %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

      
   - The LAA eNB does not need a backoff process (i.e., no picking for any random number N).
   - The LAA eNB should wait a fixed time equal to N which is set to be Nmode of the CDF of the ON times of the existing Wi-Fi.
   - No adaptation for the upper size of the CW based on the HARQ ACK feedbacks.



                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%% Programming %%%%%%%%%%%%%%%%%%%%%%%%%%%%
                          %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


   * To generate the simulation results for the previous method for different traffic loads FTP Lambda ={0.5,1.5,2.5,3.0} :

   - Use the @@@lbt-access-manager((N=Nmode Method)).cc@@@ file within 'Model' file.
   - Change the FTP Lambda as 0.5,1.5,2.5,3.0 from the file 'laa-wifi-indoor.cc'.
   - Choose the number of STAs/UEs per cell from the file 'laa-wifi-indoor.cc'.
   - Activate the following lines in the code of file 'dca-txop.cc'. The lines are:

       .AddAttribute ("CwMin", "Sets the minimum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMinCw),
                  MakeUintegerChecker<uint32_t> ())

      .AddAttribute ("CwMax", "Sets the maximum contention window.",
                   UintegerValue (),
                   MakeUintegerAccessor (&DcaTxop::SetMaxCw),
                  MakeUintegerChecker<uint32_t> ())



   - Activate the following lines in the code of file 'laa-wifi-indoor.cc'. The lines are: 
          
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMin", UintegerValue ());
   Config::Set ("/NodeList/*/DeviceList/*/$ns3::WifiNetDevice/Mac/$ns3::RegularWifiMac/DcaTxop/CwMax", UintegerValue ());

    NOTE: We added the previous lines to the 'dca-txop.cc' and 'laa-wifi-indoor.cc' files 


    - Calibrate the following line in the @@@lbt-access-manager((N=Nmode Method)).cc@@@ with the suitable values:

         return (m_rng->GetInteger (11,11)); // this is because N=Nmode=11 for lambda=0.5.


    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'run-laa-wifi-indoor-ftp.sh'.
    - Change the base_duration=120 based on the FTP Lmabda as follwing to get a total of 240 sec:


       For FTP Lambda= 0.5, the base_duration=120.
       For FTP Lambda= 1.5, the base_duration=360.
       For FTP Lambda= 2.5, the base_duration=600.
       For FTP Lambda= 3.0, the base_duration=720.

    - Change the FTP Lmabda as 0.5,1.5,2.5,3.0 for the file 'plot-laa-wifi-indoor-ftp.sh'.
































