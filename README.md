# githubaction-demo2
gopi-multi-env

We have deployed 24.4 lsrest in 23.4 environment to see if the performance issue is fixed. We are testing the same

// Source code is decompiled from a .class file using FernFlower decompiler.
package com.documentum.install.server.installanywhere.actions;
 
import com.documentum.install.server.common.services.DiServerStaticClass;
import com.documentum.install.shared.common.error.DiException;
import com.documentum.install.shared.common.services.DiDataStore;
import com.documentum.install.shared.common.services.DiLogger;
import com.documentum.install.shared.common.services.DiServices;
import com.documentum.install.shared.installanywhere.actions.InstallWizardAction;
import com.zerog.ia.api.pub.InstallException;
import com.zerog.ia.api.pub.InstallerProxy;
 
public class DiWAServerLoadServerWebcacheInfo extends InstallWizardAction {
   private String connectionString = "SERVER.DATABASE_CONNECTION";
   private String databaseConnectionVectorString = "SERVER.DATABASE_CONNECTION_VECTOR";
   private String databaseType = "SERVER.DATABASE_TYPE";
   private String databaseServerName = "";
   private String databasePort = "";
   private String databaseID = "";
   private String sybaseDatabaseServerHost = "";
   private String progressBarMsg = "Loading: Server Webcache information";
 
   public DiWAServerLoadServerWebcacheInfo() {
   }
 
   public void setup(InstallerProxy arg0) throws InstallException {
      this.setStatusText(this.progressBarMsg);
      DiDataStore context = DiServices.getGlobalContext();
      DiLogger.info(this, "The installer will obtain database information of " + this.resolveString(this.connectionString) + ".", (String[])null, (Throwable)null);
      String tempConnectionString = this.resolveString(this.connectionString).trim();
      String tempDatabaseConnectionVectorString = this.resolveString(this.databaseConnectionVectorString).trim();
      String tempDatabaseType = this.resolveString(this.databaseType).trim();
 
      try {
         if (!tempDatabaseType.equalsIgnoreCase("SQLServer") && !tempDatabaseType.equalsIgnoreCase("SYBASE") && !tempDatabaseType.equalsIgnoreCase("Postgresql")) {
            if (tempDatabaseType.equalsIgnoreCase("ORACLE")) {
               for(tempConnectionString = tempConnectionString.toUpperCase(); tempDatabaseConnectionVectorString.length() > 0 && !tempDatabaseConnectionVectorString.startsWith(tempConnectionString + "@"); tempDatabaseConnectionVectorString = tempDatabaseConnectionVectorString.substring(tempDatabaseConnectionVectorString.indexOf(";") + 1).trim()) {
               }
 
               tempDatabaseConnectionVectorString = tempDatabaseConnectionVectorString.substring(tempDatabaseConnectionVectorString.indexOf("@") + 1);
               tempDatabaseConnectionVectorString = tempDatabaseConnectionVectorString.substring(tempDatabaseConnectionVectorString.indexOf("@") + 1);
               this.databaseServerName = tempDatabaseConnectionVectorString.substring(0, tempDatabaseConnectionVectorString.indexOf("@")).trim();
               tempDatabaseConnectionVectorString = tempDatabaseConnectionVectorString.substring(tempDatabaseConnectionVectorString.indexOf("@") + 1);
               this.databasePort = tempDatabaseConnectionVectorString.substring(0, tempDatabaseConnectionVectorString.indexOf("@")).trim();
               this.databaseID = tempDatabaseConnectionVectorString.substring(tempDatabaseConnectionVectorString.indexOf("@") + 1, tempDatabaseConnectionVectorString.indexOf(";")).trim();
            }
         } else {
            String tmpDatabaseConnectionVectorString = tempDatabaseConnectionVectorString.toLowerCase();
 
            while(!tmpDatabaseConnectionVectorString.startsWith(tempConnectionString.toLowerCase() + "@")) {
               tempDatabaseConnectionVectorString = tempDatabaseConnectionVectorString.substring(tempDatabaseConnectionVectorString.indexOf(";") + 1).trim();
               tmpDatabaseConnectionVectorString = tempDatabaseConnectionVectorString.toLowerCase();
               if (tmpDatabaseConnectionVectorString.length() == 0) {
                  throw new DiException(getString("server.error.queryODBCFailed"));
               }
            }
 
            this.databaseServerName = tempDatabaseConnectionVectorString.substring(tempDatabaseConnectionVectorString.indexOf("@") + 1, tempDatabaseConnectionVectorString.indexOf(";")).trim();
         }
      } catch (DiException var12) {
         this.handleException(var12, "Failed to gather information to configure webcache.ini.", false);
      } catch (Exception var13) {
         DiLogger.info(this, var13.getMessage(), (String[])null, (Throwable)null);
         String var10002 = var13.getMessage();
         DiException tempException = new DiException(var10002 + " Please read error log " + this.resolveString("$C(SERVER,LOG_FILE_NAME)") + " for more information.");
         this.handleException(tempException, "Failed to gather information to configure webcache.ini.", false);
      } finally {
         DiServerStaticClass.setContextProperty("$C(SERVER,DATABASE_PORT)", this.databasePort.trim(), context);
         DiServerStaticClass.setContextProperty("$C(SERVER,DATABASE_ID)", this.databaseID.trim(), context);
         DiServerStaticClass.setContextProperty("$C(SERVER,DATABASE_SERVER)", this.databaseServerName.trim(), context);
         DiServerStaticClass.setContextProperty("$C(SERVER,SYBASE_DATABASE_SERVER_HOST)", this.sybaseDatabaseServerHost.trim(), context);
      }
 
   }
 
   public String getInstallStatusMessage() {
      return null;
   }
 
   public String getUninstallStatusMessage() {
      return null;
   }
 
   public long getEstimatedTimeToInstall(InstallerProxy proxy) {
      return 100L;
   }
}
 
 
15:01:32,339  INFO [main] com.documentum.install.server.installanywhere.actions.DiWAServerLoadServerWebcacheInfo - The installer will obtain database information of ORMEDSCD.MERCK.

15:01:32,340  INFO [main] com.documentum.install.server.installanywhere.actions.DiWAServerLoadServerWebcacheInfo - begin 0, end -1, length 0

15:01:32,342 ERROR [main] com.documentum.install.server.installanywhere.actions.DiWAServerLoadServerWebcacheInfo - Failed to gather information to configure webcache.ini.

com.documentum.install.shared.common.error.DiException: begin 0, end -1, length 0 Please read error log  for more information.

        at com.documentum.install.server.installanywhere.actions.DiWAServerLoadServerWebcacheInfo.setup(DiWAServerLoadServerWebcacheInfo.java:109) [installer.zip:?]

        at com.documentum.install.shared.installanywhere.actions.InstallWizardAction.install(InstallWizardAction.java:73) [installer.zip:?]

        at com.zerog.ia.installer.actions.CustomAction.installSelf(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.AAMgrBase.ak(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.ConsoleBasedAAMgr.ac(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.AAMgrBase.aj(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.AAMgrBase.runNextInstallPiece(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.ConsoleBasedAAMgr.ac(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.AAMgrBase.aj(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.AAMgrBase.runNextInstallPiece(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.ConsoleBasedAAMgr.ac(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.AAMgrBase.aj(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.AAMgrBase.runNextInstallPiece(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.ConsoleBasedAAMgr.ac(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.AAMgrBase.aj(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.AAMgrBase.runNextInstallPiece(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.ConsoleBasedAAMgr.ac(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.AAMgrBase.aj(Unknown Source) [installer.zip:?]

        at com.zerog.ia.installer.AAMgrBase.runNextInstallPiece(Unknown Source) [installer.zip:?]
 
