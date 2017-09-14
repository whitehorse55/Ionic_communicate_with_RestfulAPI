# Ionic_communicate_with_RestfulAPI

import { Injectable } from '@angular/core';
import { Observable } from 'rxjs/Observable';
import { Http, Headers, Response, RequestOptions } from '@angular/http';
import { NativeStorage } from '@ionic-native/native-storage';
import { Network } from '@ionic-native/network';
import 'rxjs/Rx';

/*
  Generated class for the RestfulAPIService provider.

  See https://angular.io/docs/ts/latest/guide/dependency-injection.html
  for more info on providers and Angular 2 DI.
*/


@Injectable()
export class RestfulAPIService {

  /**
   * API EndPoints
   */
  public BaseURL = "http://onedollar.projects.nng.bz/api/";
  //public BaseURL = "http://api.onedollarapp.biz/api/";
  public URL_COUNTRIES = "countries/";
  public URL_SIGN_UP = "users/signup/";
  public URL_SIGN_IN_FB = "users/me/";
  public URL_SIGN_IN_GP = "users/me/";
  public URL_SIGN_IN_EMAIL = "users/me/";
  public URL_GET_MY_PRODUCT = "users/me/products/";
  public URL_FORGOT_PASSWORD = "password/forget/";
  public URL_PRODUCT_LIST = "products/?page_size=20";
  public URL_GET_BET_MISSES = "users/me/bets/failed/";
  public URL_GET_BET_PROGRESS = "users/me/bets/in_progress/";
  public URL_GET_BET_WON = "users/me/bets/won/";
  public URL_BUY_BET = "bets/";
  public URL_GET_MY_BET = "users/me/bets/";
  public URL_GET_MY_PRODUCT_SELLING = "users/me/products/selling/";
  public URL_GET_MY_PRODUCT_SOLD = "users/me/products/sold/";
  public URL_GET_USER_PROFILE = "users/@@/";
  public URL_EDIT_USER_PROFILE = "users/@@/";
  public URL_GET_NOTIFICATION = "users/me/notifications/";
  public URL_GET_USER_SELLING = "users/@@/products/selling/";
  public URL_GET_USER_SOLD = "users/@@/products/sold/";
  public URL_GET_TOPUP = "transactions/";
  public URL_GET_PRODUCT_DETAIL = "products/@@/";
  public URL_POST_PRODUCT_SHARE = "products/@@/share/";
  public URL_GET_WHO_BET = "products/@@/bets/";
  public URL_RATING = "rating/";
  public URL_COMMENT = "products/@@/comments/";
  public URL_GET_CATEGORY = "categories/";
  public URL_SEARCH = "products/?search_query=@@&order_by=@@";
  public URL_SOCKET_REALTIME = this.BaseURL + ":3000/?token=@@";
  public URL_GET_MESSAGE = "users/@@/products/@@/comments/";
  public URL_POST_MESSAGE = "users/@@/comments/";
  public URL_GET_MY_SOLD = "users/me/products/sold/";
  public URL_GET_RATING = "users/@@/ratings/";
  public URL_CREATE_PRODUCT = "products/";
  public URL_UPDATE_PRODUCT = "products/@@/";
  public URL_DELETE_PHOTO = "productphotos/@@/";
  public URL_DELETE_PRODUCT = "products/@@/";
  public URL_GET_REPORT_PRODUCT = "reporttypes/product/";
  public URL_POST_REPORT_PRODUCT = "report/product/";
  public URL_GET_REPORT_USER = "reporttypes/user/";
  public URL_POST_REPORT_USER = "report/user/";
  public URL_GET_CLAIM = "claim/";
  public URL_POST_CLAIM = "claim/";
  public URL_POST_CHAT_LASTED = "users/@@/products/@@/lastread/";
  public URL_GET_LOCATION = "https://api.foursquare.com/v2/venues/search?client_id=ZPCMMIHKOID2FWWCULIAQKBFI1JA3LMTQFXH3ZYU2KEE5YOC&client_secret=JPWGI1R5T22TQ3YAIRHSAREUISAUCMI2QT2LFOWSCAJX5SPU&ll=@@&v=20151025&query=@@";
  public URL_GET_BADGE = "users/me/badges/";
  public URL_DELETE_BADGE = "users/me/badges/";
  public URL_POST_VERIFY_EMAIL = "email/verify/";
  public URL_GET_TRANSACTION = "transactions/";
  public URL_GET_REFERRER_CODE = "referrer/";
  public URL_GET_APP_SETTINGS = "settings/";
  public URL_MY_PUSHTOKEN = "users/me/pushtoken/";
  public URL_TOPUP_PACKAGES = "topuppackages/";
  public URL_GET_SETTING = "settings/";

  /**
   * Token
   */
  public token: string;
  // private _token: string;

  // set token(newToken: string) {
  //   this.nativeStorage.setItem("PREF_USER_TOKEN", newToken).then(function (data) {
  //     this._token = data as string;
  //   }
  //   );
  //   this.nativeStorage.setItem("PREF_USER_TOKEN", newToken).then(
  //     () => console.log('Stored item!'),
  //     error => console.error('Error storing item', error)
  //   );
  // }

  // get token() {
  //   if (this._token) {
  //     return this._token;
  //   }
  //   this.nativeStorage.getItem("PREF_USER_TOKEN").then(function (data) {
  //     this._token = data as string;
  //     return this._token;
  //   });
  // }


  /**
   * Network Status
   */
  public isNetworkConnect: Boolean

  checkNetwork() {
    if (this.network.type === 'None') {
      this.isNetworkConnect = false;
    } else {
      this.isNetworkConnect = true;
    }

    this.network.onConnect().subscribe(() => {
      this.isNetworkConnect = true;
    });
    this.network.onDisconnect().subscribe(() => {
      this.isNetworkConnect = false;
    });
  }


  /**
   * Life Cycle
   */
  constructor(public http: Http,
    public nativeStorage: NativeStorage,
    public network: Network) {
    // Checing Network Connnection
    this.checkNetwork();
    console.log('invoked RestfulAPIService Provider');    
  }


  /**
   * Public Methods
   */
  makeGetRequest(url: string) {
    var requestOptions = this.makeHeader(false);
    url = this.BaseURL + url;

    var response = this.http.get(url, requestOptions)
      .map(res => res.json())
      .catch(this.handleError);

    return response;
  }

  makeGetRequestWithToken(url: string) {
    var requestOptions = this.makeHeader(true);
    url = this.BaseURL + url;

    var response = this.http.get(url, requestOptions)
      .map(res => res.json())
      .catch(this.handleError);

    return response;
  }

  makePostRequest(url: string, parameters: any) {
    // var requestOptions = this.makeHeader(false);
    // var params = JSON.stringify(parameters);
     var headers = new Headers();
    var requestOptions = new RequestOptions({ "headers": headers });

    console.log("header=======>", requestOptions);
    console.log("parameters=======>", parameters);
    url = this.BaseURL + url;

    var response = this.http.post(url, parameters, requestOptions)
      .map(res => res.json())
      .catch(this.handleError);

    return response;
  }

  makePostRequestWithToken(url: string, parameters: any) {
    var requestOptions = this.makeHeader(true);
    var params = JSON.stringify(parameters);
    url = this.BaseURL + url;

    var response = this.http.post(url, params, requestOptions)
      .map(res => res.json())
      .catch(this.handleError);

    return response;
  }

  makeDelete(url: string) {
    var requestOptions = this.makeHeader(false);
    url = this.BaseURL + url;

    var response = this.http.delete(url, requestOptions)
      .map(res => res.json())
      .catch(this.handleError);
  }

  makeDeleteWithToken(url: string, parameters: any) {
    var requestOptions = this.makeHeader(true);
    url = this.BaseURL + url;

    var response = this.http.delete(url, requestOptions)
      .map(res => res.json())
      .catch(this.handleError);
  }

  makePatchWithToken(url: string, token: string, parameters: any) {
    var headers = new Headers();
    headers.append('Authorization', "Token " + token);
    headers.append("X-HTTP-Method-Override", "PATCH");
    var requestOptions = new RequestOptions({ "headers": headers });
    
    url = this.BaseURL + url;

    var response = this.http.patch(url, parameters, requestOptions)
      .map(res => res.json())
      .catch(this.handleError);

    return response;
  }

  /**
   * Private Methods
   */
  handleError(error) {
    console.error(error);
    return Observable.throw(error.json().error || 'Server error');
  }

  makeHeader(isRequiredToken: Boolean): RequestOptions {
    var headers = new Headers();
    headers.append('Content-Type', 'application/json');

    if (isRequiredToken) {
      headers.append('Authorization', "Token " + this.token);
      console.log("restful token=====>", this.token);
    }
    var requestOptions = new RequestOptions({ "headers": headers });
    return requestOptions;
  }

}
