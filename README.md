# Projet-C-E-Bank
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#define CMAX 20
int choix,x;
typedef enum
    {
        false,
        true
    } Bool; 
typedef  struct SClient
    {
        char nom[CMAX];
        char prenom[CMAX];
        int id_client;
    } SClient;
typedef struct Date
    {
        int jour;
        int mois;
        int an;
    } Date;
typedef struct SCompte
    {
        int id_compte;
        int id_client;
        Date date_compte;
        int solde;
    } SCompte;

void gerer_client(int a)
    {
        switch(a)
            {
                case 1: //Ajout client//
                    {
                        SClient client ;
                        FILE *file;
                        printf("Donnez id_client: ");
                        scanf("%d",&client.id_client);
                        printf("Donner le nom du client: ");
                        scanf("%s",client.nom);
                        printf("Donner son prénom");
                        scanf("%s",client.prenom);
                        file=open("client.txt","a");
                        if (file==NULL)
                            {
                                exit(1);
                            }
                        fwrite(&client,sizeof(client),1,file);
                        if (fwrite!=0)
                            {
                                printf("Client ajouté avec succès");
                            }
                        fclose(file);
                        break;
                    }
                case 2: //Modification client//
                    {
                        SClient client;
                        SClient nv_client;
                        FILE *file;
                        int nbclient;
                        printf("Code client à modifier");
                        scanf("%d",&nv_client.id_client);
                        printf("Nouveau Nom : ");
                        fgets(nv_client.nom,sizeof(nv_client.nom),stdin);
                        printf("Nouveau Préom : ");
                        fgets(nv_client.prenom,sizeof(nv_client.prenom),stdin);
                        file=fopen("client.txt","r");
                        fseek(file,0,SEEK_END);
                        nbclient=ftell(file);
                        rewind(file);
                        nbclient++;
                        SClient Clients[nbclient];
                        for (int i=0;i<nbclient;i++)
                            {    
                                fread(&client,sizeof(SClient),1,file);
                                Clients[i]=client;
                            }
                        fclose(file);
                        remove(file);
                        FILE *nf;
                        nf=fopen("nv.txt","a");
                        for(int i=0;i<nbclient;i++)
                            {
                                if(nv_client.id_client==Clients[i].id_client)
                                    {
                                        fwrite(&nv_client,sizeof(Clients[i]),1,nf);
                                    }
                                else
                                    {
                                        fwrite(&Clients[i],sizeof(Clients[i]),1,nf);
                                    }
                            }
                        fclose(nf);
                        rename("nv.txt","client.txt"); 
                        break;
                    }
                case 3://Supprimer client//
                    {
                        SClient client;
                        int code;
                        FILE *file;
                        int nbclient;
                        printf("Code client à supprimer: ");
                        scanf("%d",&code);
                        file=fopen("client.txt","r");
                        fseek(file,0,SEEK_END);
                        nbclient=ftell(file);
                        rewind(file);
                        nbclient++;
                        SClient Clients[nbclient];
                        for (int i=0;i<nbclient;i++)
                            {    
                                fread(&client,sizeof(SClient),1,file);
                                Clients[i]=client;
                            }
                        fclose(file);
                        remove(file);
                        FILE *nf;
                        nf=fopen("nv.txt","a");
                        for(int i=0;i<nbclient;i++)
                            {
                                if(Clients[i].id_client!=code)
                                    {
                                        fwrite(&Clients[i],sizeof(Clients[i]),1,nf);
                                    }
                            }
                        fclose(nf);
                        rename("nv.txt","client.txt"); 
                        break;
                    }
                case 4://Afficher liste de clients//
                    {
                        FILE *file;
                        SClient client;
                        int nbclient;
                        file=fopen("client.txt","r");
                        fseek(file,0,SEEK_END);
                        nbclient=ftell(file);
                        rewind(file);
                        SClient Clients[nbclient];
                        for (int i=0;i<nbclient;i++)
                            {    
                                fread(&client,sizeof(SClient),1,file);
                                Clients[i]=client;
                                printf("\nClient: %s %s",Clients[i].nom,Clients[i].prenom);
                                printf("\nIdentifiant du client est : %d",Clients[i].id_client);
                                printf("\n");
                            }
                        fclose(file);
                        break;
                    }
                case 5://Retour//
                    {
                        menu();
                        break;
                    }
                default://Retour//
                    {
                        printf("Entrez un choix valide");
                        menu();
                        break;
                    }
            }
    }
void gerer_comptes(int a)
    {
        switch(a)
            {
                case 1://Ajout compte//
                    {
                        SCompte compte,compte0 ;
                        FILE *file;
                        FILE *fl;
                        int t=0;
                        do                      
                            {
                                printf("Donnez id_compte: ");
                                scanf("%d",&compte.id_compte);
                                fl=fopen("compte.txt","r");
                                while(fread(&compte0,sizeof(SCompte),1,fl)&& t==0)
                                    {
                                        if (compte0.id_compte==compte.id_compte)
                                            {
                                                t==1;
                                            }
                                    }
                                if (t==1)
                                    {
                                        printf("Compte existant\n");
                                        continue;
                                    }
                                
                            }while(t==1);
                        fclose(fl);            
                        printf("Donner l'identifiant du client: ");
                        scanf("%d",&compte.id_client);
                        printf("Donnez le jour: ");
                        scanf("%d",&compte.date_compte.jour);
                        printf("Donnez le mois: ");
                        scanf("%d",&compte.date_compte.mois);
                        printf("Donnez l'année: ");
                        scanf("%d",compte.date_compte.an);
                        printf("Donnez le solde");
                        scanf("%d",compte.solde);
                        file=open("compte.txt","a");
                        if (file==NULL)
                            {
                                exit(1);
                            }
                        fwrite(&compte,sizeof(compte),1,file);
                        if (fwrite!=0)
                            {
                                printf("Compte ajouté avec succès");
                            }
                        fclose(file);
                        break;
                    }
                case 2://Recherche//
                    {
                        SCompte compte;
                        FILE *file;
                        int code;
                        printf("Donnez le code du compte a rechercher");
                        scanf("%d",&code);
                        file=fopen("compte.txt","r");
                        int t=0;
                        while(feof(file)==0 && t==0)
                            {
                                fread(&compte,sizeof(SCompte),1,file);
                                if (code==compte.id_compte)
                                    {
                                        printf("\nCode compte: %d",code);
                                        printf("\nIdentifiant client: %d",compte.id_client);
                                        printf("\nDate de création: %d / %d / %d ",compte.date_compte.jour,compte.date_compte.mois,compte.date_compte.an);
                                        printf("\nSolde: %d",compte.solde);
                                        t==1;
                                    }
                            }
                        if (eof(file))
                            {
                                printf("Compte introuvable");
                            }
                        fclose(file);     
                        break;       
                    }
                case 3://Supprimer//
                    {
                        SCompte compte;
                        int code;
                        FILE *file;
                        int nbcompte;
                        printf("Code compte à supprimer: ");
                        scanf("%d",&code);
                        file=fopen("compte.txt","r");
                        fseek(file,0,SEEK_END);
                        nbcompte=ftell(file);
                        rewind(file);
                        nbcompte++;
                        SCompte Comptes[nbcompte];
                        for (int i=0;i<nbcompte;i++)
                            {    
                                fread(&compte,sizeof(SCompte),1,file);
                                Comptes[i]=compte;
                            }
                        fclose(file);
                        remove(file);
                        FILE *nf;
                        nf=fopen("nv.txt","a");
                        for(int i=0;i<nbcompte;i++)
                            {
                                if(Comptes[i].id_compte!=code)
                                    {
                                        fwrite(&Comptes[i],sizeof(Comptes[i]),1,nf);
                                    }
                            }
                        fclose(nf);
                        rename("nv.txt","compte.txt"); 
                        break;           
                    }
                case 4://Affichage comptes//
                    {
                        SCompte compte;
                        FILE *file;
                        file=fopen("compte.txt","r");
                        while(feof(file)==false)
                            {
                                fread(&compte,sizeof(SCompte),1,file);
                                printf("\nCode compte: %d",compte.id_compte);
                                printf("\nIdentifiant client: %d",compte.id_client);
                                printf("\nDate de création: %d / %d / %d ",compte.date_compte.jour,compte.date_compte.mois,compte.date_compte.an);
                                printf("\nSolde: %d",compte.solde);
                                printf("\n\n");
                            }
                        fclose(file);
                        break;
                    }
                case 5://Retour//
                    {
                        menu();
                        break;
                    }
                default://Retour//
                    {
                        menu();
                        break;
                    }
            }
    }
void gerer_transactions(int a)
    {
        switch(a)
            {
                case 1://Retrait//
                    {
                        SCompte compte;
                        int code,montant;
                        FILE *file;
                        int nbcompte;
                        printf("Code compte: ");
                        scanf("%d",&code);
                        file=fopen("compte.txt","r");
                        fseek(file,0,SEEK_END);
                        nbcompte=ftell(file);
                        rewind(file);
                        nbcompte++;
                        SCompte Comptes[nbcompte];
                        for (int i=0;i<nbcompte;i++)
                            {    
                                fread(&compte,sizeof(SCompte),1,file);
                                Comptes[i]=compte;
                            }
                        fclose(file);
                        remove(file);
                        FILE *nf;
                        nf=fopen("nv.txt","a");
                        int t=0;
                        for(int i=0;i<nbcompte;i++)
                            {
                                if(Comptes[i].id_compte==code)
                                    {
                                        do
                                            {
                                                printf("Donnez le montant de retrait: ");
                                                scanf("%d",&montant);
                                                if (montant>Comptes[i].solde)
                                            
                                                printf("Solde insuffisant!!");
                                            }while (Comptes[i].solde<montant);  
                                        Comptes[i].solde-=montant;
                                        t=1;
                                    }    
                                fwrite(&Comptes[i],sizeof(Comptes[i]),1,nf);
                                    
                            }
                        fclose(nf);
                        rename("nv.txt","compte.txt"); 
                        if (t==0)
                            {
                                printf("Compte inexistant");
                            }
                        else
                            {
                                for (int i=0;i<nbcompte;i++)
                                    {
                                        printf("\nCode compte: %d",code);
                                        printf("\nIdentifiant client: %d",Comptes[i].id_client);
                                        printf("\nDate de création: %d / %d / %d ",Comptes[i].date_compte.jour,Comptes[i].date_compte.mois,Comptes[i].date_compte.an);
                                        printf("\nSolde: %d",Comptes[i].solde);
                                        printf("\n\n");
                                    }
                            }  
                        break;         
                    }
                    
                case 2://Versement//
                    {
                        SCompte compte;
                        int code,montant;
                        FILE *file;
                        int nbcompte;
                        printf("Code compte: ");
                        scanf("%d",&code);
                        file=fopen("compte.txt","r");
                        fseek(file,0,SEEK_END);
                        nbcompte=ftell(file);
                        rewind(file);
                        nbcompte++;
                        SCompte Comptes[nbcompte];
                        for (int i=0;i<nbcompte;i++)
                            {    
                                fread(&compte,sizeof(SCompte),1,file);
                                Comptes[i]=compte;
                            }
                        fclose(file);
                        remove(file);
                        FILE *nf;
                        nf=fopen("nv.txt","a");
                        int t=0;
                        for(int i=0;i<nbcompte;i++)
                            {
                                if(Comptes[i].id_compte==code)
                                    {
                                        Comptes[i].solde+=montant;                                   
                                        t=1;
                                    }    
                                fwrite(&Comptes[i],sizeof(Comptes[i]),1,nf);
                                    
                            }
                        fclose(nf);
                        rename("nv.txt","compte.txt"); 
                        if (t==0)
                            {
                                printf("Compte inexistant");
                            }  
                        else
                            {
                                for (int i=0;i<nbcompte;i++)
                                    {
                                        printf("\nCode compte: %d",code);
                                        printf("\nIdentifiant client: %d",Comptes[i].id_client);
                                        printf("\nDate de création: %d / %d / %d ",Comptes[i].date_compte.jour,Comptes[i].date_compte.mois,Comptes[i].date_compte.an);
                                        printf("\nSolde: %d",Comptes[i].solde);
                                        printf("\n\n");
                                    }
                            }   
                        break;                                       
                    }
                case 3://Retour//
                    {
                        menu();
                        break;
                    }
                default://Retour//
                    {
                        menu();
                        break;
                    }
            }

    }

void sous_menu(int a)
    {
        switch(a)
            {
                case 1://Client//
                    {
                        printf("1-Ajouter\n");
                        printf("2-Modifier\n");
                        printf("3-Supprimer\n");
                        printf("4-Afficher\n");
                        printf("5-Retour\n");
                        printf("6-choisir un sous menu: ");
                        scanf("%d",&x);
                        gerer_client(x);
                        break;
                    }
                case 2://Comptes//
                    {
                        printf("1-Ajouter\n");
                        printf("2-Rechercher\n");
                        printf("3-Supprimer\n");
                        printf("4-Afficher la liste des comptes\n");
                        printf("5-Retour\n");
                        printf("6-choisir un sous menu: ");
                        scanf("%d",&x);
                        gerer_comptes(x);
                        break;
                    }
                case 3://Transactions//
                    {
                        printf("1-Retrait d'argent\n");
                        printf("2-Versement d'argent\n");
                        printf("3-Retour\n");
                        printf("4-choisir un sous menu: ");
                        scanf("%d",&x);
                        gerer_transactions(x);
                        break;
                    }
                case 4://Quitter//
                    {
                        return 0;
                        break;
                    }
                default://Retour//
                    {
                        printf("Donnez un choix valide!!\n");
                        menu();
                        break;
                    }
            }
    }
void menu()
    {
    printf("1-Gestion des clients\n");
    printf("2-Gestion des comptes\n");
    printf("3-Gestion des transactions\n");
    printf("4-Quitter\n");
    printf("Choisir le menu ");
    scanf("%d",&choix);
    sous_menu(choix);
    }

int main()
    {
        menu();
        return 0;
    }
