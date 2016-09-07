<?php

function getValues ($value){
    
    //mysql_real_escape_string
    $value = mysql_real_escape_string($value);
    
    //trim - sol sağ boşluk alma
    $value = trim($value);
    
    return $value;
    
}

function postValues($value){
    
       //mysql_real_escape_string
    $value = mysql_real_escape_string($value);
    
    //trim - sol sağ boşluk alma
    $value = trim($value);
    
    return $value;    
    
}


//üst kategori bilgisini alır
function parentKategoriBul ($parentID) {
    
    $query_rsParentKategori = "SELECT * FROM kategori  WHERE KategoriID='$parentID'";
    $rsParentKategori = mysql_query($query_rsParentKategori);
    $row_rsParentKategori = mysql_fetch_object($rsParentKategori);
    
    return $row_rsParentKategori->Kategori;
}

//bu fonksiyon bir kategorinin içinde ürün varmı kontrol eder.
function UrunVarmi ($kategoriID) {
     $query= "SELECT * FROM urun WHERE KategoriID= '$kategoriID'";
     $result = mysql_query($query);
     $deger = mysql_num_rows($result);
    
    return $deger;
}

//bu fonksiyon üyenin giriş yapıp yapmadığını tutuyor.
//true veya false değeri döndürüyor
function GirisVarmi(){
    
    
    if(empty($_SESSION['Uye']['UyeID']) || empty($_SESSION['Uye']['KullaniciAdi']) || empty($_SESSION['Uye']['Eposta']) || empty($_SESSION['Uye']['SeviyeID'])      )
{
    
    $girisVarmi = false;
    
    
}else{
    
    $girisVarmi = true;
    
}
    return $girisVarmi;
    
}//girisVarmi () fns sonu

//bu fonksiyon üyenin o sayfada yetkili olup olmadığını tutar
//true veya false değeri döndürür
function yetkiVarmi($seviyeID=6,$yetki=6,$uyeID='',$modulID='',$alan='') {
    
    if($seviyeID<=$yetki){
        //1. katman üye seviyesi yeterli
  
        
        //2.katman o sayfada işlem yetkisi varmı ?
        $query = "SELECT $alan  FROM modul_uye WHERE UyeID='$uyeID' AND  ModulID='$modulID' AND $alan='1' ";
        $result = mysql_query($query);
        $num_rows=mysql_num_rows($result);
        
        if($num_rows!=0){
            
                  $yetkiVarmi = true;
            
        }else{
                $yetkiVarmi = false;
            
        }
        
        
        
    }elseif($yetki>=$seviyeID){
        $yetkiVarmi = false;
        
    }
    
    return $yetkiVarmi;
 
}//yetkiVarmi fns sonu


//php self ile kendine dönme
function phpSelf (){
    
    return $_SERVER['PHP_SELF'];
    
}


//fonksiyon modül aktifmi diye bakarı geriye true, false değeri döndürür
function modulAktifmi($dizinAdi){
    
        $query_rsModul = "SELECT ModulID, ModulAktif, ModulDizin FROM modul WHERE ModulDizin='$dizinAdi' AND ModulAktif=1";
        $rsModul  = mysql_query($query_rsModul);
        $num_rsModul = mysql_num_rows($rsModul);
       // $aktif = empty($sonuc)?0:1;
        
       //return $aktif;
        
        return         $num_rsModul ;
        
       // return $query_rsModul;
    
}
    

?>

