# Oracle DB

  Oracle on-premises solutions, such as Oracle Database, do not have a built-in IAM (Identity and Access Management) system similar to cloud services like Oracle Cloud Infrastructure (OCI). However, Oracle provides solutions for on-premises IAM through its Oracle Identity and Access Management (IAM) Suite.
  
  ## Oracle Identity and Access Management (IAM) Suite for On-Premises:
  
  1. Oracle Identity Manager (OIM):
  
     *  Oracle Identity Manager is a component of the Oracle IAM Suite that provides identity lifecycle management. It helps organizations manage user identities and their access privileges.
    
  2. Oracle Access Manager (OAM):
     *  Oracle Access Manager provides access management and single sign-on functionality. It enables organizations to control access to applications and resources based on user roles and policies.
  
  3. Oracle Internet Directory (OID):
     *  Oracle Internet Directory is a directory service that stores and manages identity-related information. It is often used as the LDAP directory for Oracle IAM solutions.
  
  4.  Oracle Virtual Directory (OVD):
    *  Oracle Virtual Directory is a lightweight directory access protocol (LDAP) service that provides a virtual view of identity data from multiple sources, helping to integrate and centralize identity information.
  
   ## Basic Steps for On-Premises IAM Role Setup:
      1. Installation and Configuration:
          * Install and configure the Oracle Identity and Access Management Suite components (OIM, OAM, OID, OVD) on your on-premises infrastructure.
  
    2.   Define Roles and Policies:
        *  Within Oracle Identity Manager, define roles that represent specific job functions or responsibilities.
        *  Associate users with these roles based on their responsibilities.
  
    3.  Set Access Policies:
        *  Use Oracle Access Manager to set up access policies that determine which resources (applications, databases, etc.) users with specific roles can access.
  
    4.  LDAP Integration:
       *  Integrate Oracle Internet Directory with Oracle Identity Manager to store and manage identity-related information centrally.
  
    5.  Single Sign-On (SSO):
      *   Implement single sign-on capabilities using Oracle Access Manager to allow users to authenticate once and access multiple resources seamlessly.
  
    6.  Monitoring and Auditing:
      *   Set up monitoring and auditing features to track user activities and ensure compliance with security policies.
  
  ## Example Role Assignment:
        For example, within Oracle Identity Manager, you might create a role called "Database Administrator" and associate it with users who need administrative access to Oracle databases. The access policies within Oracle Access Manager would then control what actions those users can perform on the databases.
  
        Remember that the specifics of role creation, policy definition, and integration may vary based on the versions of Oracle IAM Suite components you are using. Always refer to the official documentation for the version you have installed for detailed instructions.

# Microsoft SQL Server
  Microsoft SQL Server on-premises does not have a built-in Identity and Access Management (IAM) system like cloud services. Instead, it relies on the Windows operating system security and Microsoft SQL Server's own security features for user authentication and authorization.
  
  Here are the key components and concepts related to security and access management in Microsoft SQL Server on-premises:
  
    1. Windows Authentication:
        * SQL Server can be configured to use Windows Authentication mode, where users are authenticated against the Windows operating system. This can involve Active Directory for domain-based authentication.
    2. SQL Server Logins:
        * SQL Server logins are server-level principals associated with a SQL Server instance. These logins can be mapped to Windows user accounts or be created directly within SQL Server.
    3. Database Users:
        * Database users are created within a specific database and are associated with a SQL Server login. Permissions for accessing objects within the database are assigned to these database users.
    4. Roles:
       * SQL Server supports fixed server roles and fixed database roles, as well as user-defined roles. Roles are used to group users together and assign permissions collectively.
    
  Example Scenario:
    Suppose you want to create a role for database administrators on a specific database:
  
    1. Create a SQL Server Login:
       * Create a SQL Server login for the user or group that needs access to the SQL Server instance. This can be a Windows user or group if using Windows Authentication.
          CREATE LOGIN [YourDomain\YourUser] FROM WINDOWS;
    
    2. Create a Database User:
      *  Create a user within the specific database and map it to the SQL Server login.
          USE YourDatabase;
          CREATE USER [YourUser] FOR LOGIN [YourDomain\YourUser];
     
    3.  Create a Database Role:
      *  Create a database role for database administrators.
          USE YourDatabase;
          CREATE ROLE db_admins;
    4. Assign Permissions:
      *  Grant necessary permissions to the database role.
         USE YourDatabase;
         GRANT SELECT, INSERT, UPDATE, DELETE ON dbo.YourTable TO db_admins;
        
   5.  Add User to Role:
      *  Add the user to the database role.
        USE YourDatabase;
        ALTER ROLE db_admins ADD MEMBER [YourUser];
    
This is a simplified example, and the exact steps may vary based on your specific requirements and SQL Server version. Always refer to the official Microsoft SQL Server documentation for detailed instructions and best practices for securing your SQL Server on-premises environment.
