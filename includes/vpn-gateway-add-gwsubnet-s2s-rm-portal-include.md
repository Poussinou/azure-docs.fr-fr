1. Dans le portail, accédez au réseau virtuel pour lequel vous souhaitez créer une passerelle de réseau virtuel.
2. Dans la section**Paramètres** du panneau de votre réseau virtuel, cliquez sur **Sous-réseaux** pour développer le panneau Sous-réseaux.
3. Dans le panneau **Sous-réseaux**, cliquez sur **+ Sous-réseau de passerelle** en haut. Cette opération ouvre le panneau **Ajouter un sous-réseau** . 
   
    ![Ajouter un sous-réseau de passerelle](./media/vpn-gateway-add-gwsubnet-s2s-rm-portal-include/addgwsubnet.png "Ajouter un sous-réseau de passerelle")
4. Le **Nom** de votre sous-réseau est automatiquement rempli avec la valeur « GatewaySubnet ». Cette valeur est nécessaire pour qu’Azure puisse reconnaître le sous-réseau en tant que sous-réseau de passerelle. Ajustez les valeurs de **plage d’adresses** renseignées automatiquement pour qu’elles correspondent à la configuration requise.

    ![Ajout du sous-réseau](./media/vpn-gateway-add-gwsubnet-s2s-rm-portal-include/gwsubnet.png "Ajout du sous-réseau")
5. Cliquez sur **OK** en bas du panneau pour créer le sous-réseau.