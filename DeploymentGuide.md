Part of **Trasnport2.Customers** Split we are migrating all the customers in existing Trasnport2.Customers to new application each with following naming standard.

**Transport2.External.XXXX** (XXX stands for the customer name.)

  Backup of existing Artifacts: 

1. Before undeploying the existing artifacts ,take the backup of dll’s & Bindings of Transport2.Cusotmers from the production server.

      * DLL  Path:  C:\Windows\Microsoft.NET\assembly\GAC_MSIL

2. Paste the copied dll’s and binding file in below path 

    *  \\fs_mbtransport\biztalk\Backup\Production\30-09-2019\Transport2.Customers


**Deployment Steps:**
 1.Check there should be no running instances of Transport2.Customers

 2.Disable the below artifacts sequentially.

 		  ReceivePorts:
       	   	* AlfaLaval.Transport.Transport.rcvInstruction
            	* Sandvik.Transport.Transport.rcvInstruction.EDI

         **SendPorts** :
    			   * AEB.Transport.Transport.SendIFTSTAD97A.EDI           
     			   * AlfaLaval.Transport.Transport.SendIFTSTAD97A.EDI
    			   * Sandvik.Transport.Transport.SendIFTSTAD97A.EDI
    		      * Swep.Transport.Transport.SendIFTSTAD97A.EDI
               
        **Orchestrations**  :
               * Transport2.Customers.AEB.Orchestrations.orcPostAEBStatus
               * Transport2.Customers.AlfaLaval.Orchestrations.orcPostAlfaLavalStatus
               * Transport2.Customers.Sandvik.Orchestrations.orcPostSandvikStatus
               * Transport2.Customers.Swep.Orchestrations.orcPostSwepStatus

3) After disabling, remove the below artifacts manually from Biztalk Admin Console in order                                              
    **Orchestrations** :
            * Transport2.Customers.AEB.Orchestrations.orcPostAEBStatus
            * Transport2.Customers.AlfaLaval.Orchestrations.orcPostAlfaLavalStatus
            * Transport2.Customers.Sandvik.Orchestrations.orcPostSandvikStatus
            * Transport2.Customers.Swep.Orchestrations.orcPostSwepStatus

	 **SendPorts** :
            * AEB.Transport.Transport.SendIFTSTAD97A.EDI           
	         * AlfaLaval.Transport.Transport.SendIFTSTAD97A.EDI
	         * Sandvik.Transport.Transport.SendIFTSTAD97A.EDI
            * Swep.Transport.Transport.SendIFTSTAD97A.EDI

     **ReceivePorts** :
            * AlfaLaval.Transport.Transport.rcvInstruction
            * Sandvik.Transport.Transport.rcvInstruction.EDI

     **Maps** :
            * Transport2.Customers.AEB.Transforms.mapEventReportV10R0iToAEBIftstaD97A                                                               * Transport2.Customers.AlfaLaval.Transforms.mapAlfaLavalIftminD97AToBringXmlV13R0i                                                       * Transport2.Customers.Sandvik.Transforms.mapSandvikIftminD97AToBringXmlV13R0i
            * Transport2.Customers.Transforms.mapTransportV13R0itoCustomerJobV10R1
            * Transport2.Customers.Transforms.mapEventReportV10R0iToIftstaD97A

     *Pipelines** :  
            * Transport2.Customers.AlfaLaval.Pipelines.rcvEDIregexp

     **Schemas** :
            * Transport2.Customers.Schemas.schEFACT_D97A_IFTSTA

**Deploy the New applications:** 

   The new applications should be deployed in the order as below using ccnet portal.

      1. Transport.Common.External
      2. Transport2.External.AEB
      3. Transport2.External.AlfaLaval
      4. Transport2.External.Swep
      5. Transport2.External.Sandvik

After deploying start all the applications .

**RollBackSteps :**

   1.   Uninstall the failed application(Trasnport2.External.XXX) from Control Panel.
   2.   Remove the Application(Trasnport2.External.XXX)  from Biztalk Console.
   3.   Gac the backup  dll’s of failed application and using add resource add the dll’s in Trasnport2.Cusotmers 
   4.   Stop the Transport2.Customers application
   5.   Import the backup bindings to Transport2.Customers.
   6.   Restart the respective host instances .
   7.   Start Transport2.Customers.
