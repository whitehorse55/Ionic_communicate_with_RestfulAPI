# Ionic_communicate_with_RestfulAPI
How To use this module
//////////////////////////////////////////////////////////example//////////////////////////////////////////////////////////////
  onSubmit()
  {
  
    let newInfo = new FormData();
    
    newInfo.append("first_name", this.userInfo.firstname);
    
    newInfo.append("last_name", this.userInfo.lastname);
    
    newInfo.append("email", this.userInfo.email);
    
    newInfo.append("phone", this.userInfo.mobile);
    
    newInfo.append("street1", this.userInfo.street1);
    
    newInfo.append("street2", this.userInfo.street2);
    
    newInfo.append("postal_code", this.userInfo.postalcode);
    
    newInfo.append("province", this.userInfo.province);
    
    newInfo.append("city", this.userInfo.city);
    
    newInfo.append("about", "this is the test data");
    
    // newInfo.append("country", this.userInfo.country.id);
    

    let url = this.API.URL_GET_USER_PROFILE.replace("@@", this.userId.toString());

    if (this.avatarFilePath != "") {
      this.avatarFilePath = "";
      newInfo.append("avatar", this.avatar);
      console.log("avatar data=====>", this.avatar);
    }
      
    
    this.API.makePatchWithToken(url, this.token, newInfo).subscribe(
      data => {
        this.setUserInfo();
        this.alert.showAlert("Update success.", "");
      },
      error => {
        console.log("error", error);
        this.alert.showAlert("Update failed.", error);
      }
    )
  }
