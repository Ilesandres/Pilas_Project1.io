
function options(){
    let text=document.getElementById("forms");
    text.innerHTML='<button onclick="retirar()">Retirar</button>  '
    text.innerHTML+='<button onclick="consignar()">consignar</button>  '
    text.innerHTML+='<button onclick="His()">Historial</button>  '
    text.innerHTML+='<div id="formV"></div>'

    
}
options();



let datos=[];
let movimientos=[];
let consMay=0;
let retMen=0;

class pila {

    constructor() {
        this.pila = {};
        this.index = 0;
    }
    apilar(user, dinero, name, edad) {
        this.pila[this.index] = user, dinero,name, edad;
        this.index++;
        return this.pila;
    }
    desapilar() {
        this.index--;
        const aux = this.pila[this.index];
        delete this.pila[this.index];
        return aux;

    }
    peek() {
        this.pila[this.index - 1];
    }
    zize() {
        this.index;

    }
    retirar(user, monto){
        let aux2=0;
        for(let i=0; i<this.index; i++){
            let aux=0;
           
            if(user==this.pila[i].user){
                
             aux=this.pila[i].dinero-monto;
             this.pila[i].dinero=aux;
             

                console.log(this.pila[i])
                console.log("saldo actual "+aux)
                alert("retiro exitoso");
                aux2=1;
                break

            }

        }
        if(aux2==0){
            alert("verifique los datos");
        }

    }
    consignar(user,monto,cuenta){
        let aux2=0;
        let aux=0;
         let aux3=0;
         let aux4=0;
        for(let i=0; i<this.index; i++){
           
            if(user==this.pila[i].user){
                for(let j=0; j<this.index; j++){
                     if(cuenta==this.pila[j].cuenta){
                        if(this.pila[j].cuenta==this.pila[i].cuenta){
                            alert("estas trando de mandar dindero a tu propia cuenta, lo cual o se puede rectifica los datos")
                            break
                        }else{
                            aux=this.pila[i].dinero-monto;
                            this.pila[i].dinero=aux;
                            console.log(this.pila[i])
                             console.log("saldo actual "+aux)
                            aux3=this.pila[j].dinero+monto;
                            this.pila[j].dinero=aux3;
                            console.log("cuenta de destino")
                            console.log(this.pila[j]);
                            alert("envio exitoso");
                            aux4=1;
                            break
                        }
                        

                    }
                    
                }
                
                aux2=1;

            }

        }
        if(aux2==0){
            alert("verifique los datos");
        }
        if(aux4==0){
            alert("no existe el numero de cuenta por favor rectifica los datos")
        }
    }

    imprimir() {
        console.log(this.pila);
        console.log(this.pila[0].name);
        for (let i = 0; i < this.index; i++) {
            console.log(this.pila[i].name);
        }

    }

    historial(dato, movimiento){
    
    datos.push(dato);
    movimientos.push(movimiento);
    //prom
    let prom1=0;
    let prom=0;
    for(let i=0; i<datos.length; i++){
        prom1=datos[i]+prom1;
    }
    prom=prom1/datos.length;
    console.log("el promedio de movimiento es= "+prom);

// conteo de retirar y consignar 
    let ret=0;
    let cons=0;
    for(let i=0; i<movimientos.length; i++){
        if(movimientos[i]=="retirar"){
            ret++;
        }else if(movimientos[i]=="consignar"){
            cons++;
        }
    }
    

    //consignacion mayor y retiro menor
    let aux5=0;
    for(let i=0; i<movimientos.length; i++){
        if(movimientos[i]=="retirar"){
            aux5=i;
        }

    }
    retMen=datos[aux5];
    for(let i=0; i<datos.length; i++){
        if(movimientos[i]=="retirar"){
            if(datos[i]<retMen){
                retMen=datos[i];
            }
        }else if(movimientos[i]=="consignar"){
            if(datos[i]>consMay){
                consMay=datos[i];
            }

        }
    }
    

    return{
        promedio: prom,
        retiros: ret,
        consignaciones: cons,
        consMayor:consMay,
        retiroMen:retMen
    }
    
    }
    



}

const pilaCajero = new pila;
pilaCajero.apilar({user:'Andres', dinero: 10000000, name:'Andres Iles', edad:'18',cuenta:'3202573'})
pilaCajero.apilar({user:'Carlos', dinero: 1000000, name:'Carlos Martines', edad:'20',cuenta:'323425'})
pilaCajero.apilar({user:'Crsitian', dinero: 900000, name:'Cristian Macias', edad:'22', cuenta:'3227542'})
pilaCajero.apilar({user:'David', dinero: 900000, name:'David Mueses', edad:'22', cuenta:'3152832'})
pilaCajero.apilar({user:'Duvan', dinero: 900000, name:'Duvan semanate', edad:'22', cuenta:'328255'})
pilaCajero.apilar({user:'Jhonatan', dinero: 900000, name:'Jhonatan Murillo', edad:'22', cuenta:'245366'})



function retirar(){
let text=document.getElementById("formV");
text.innerHTML='<br>'
text.innerHTML+="<form action='POS'><label for='#usuario'>Usuario</label><input type='text' id='usuario' placeholder='usuario'><br><label for='#monto'>valor</label><input type='number' id='monto' placeholder='valor'></form>";
text.innerHTML+='<button onclick="ejecutar()">retirar</button>'

}

function ejecutar(){
    let user=document.getElementById("usuario").value;
    let monto=parseFloat(document.getElementById("monto").value);

    console.log(user+" df "+monto)

    pilaCajero.retirar(user, monto);
    pilaCajero.historial(monto, "retirar");

}

function consignar(){
    let text=document.getElementById("formV");
    text.innerHTML='<br>'
    text.innerHTML+="<form action='POS'><label for='#usuario'>Usuario</label><input type='text' id='usuario' placeholder='usuario'><br><label for='#monto'>valor</label><input type='number' id='monto' placeholder='valor a eviar'> <br><label for='#cuenta'>N°Cuenta a enviar</label><input type='number' id='cuenta' placeholder='N° Cuenta'></form>";
    text.innerHTML+='<button onclick="enviar()">enviar</button>'
}

function enviar(){
    let user=document.getElementById("usuario").value;
    let monto=parseFloat(document.getElementById("monto").value);
    let cuenta=document.getElementById("cuenta").value;

    pilaCajero.consignar(user, monto, cuenta);
    pilaCajero.historial(monto, "consignar");

}

function His(){
    let funcion=pilaCajero.historial();
    let text=document.getElementById("formV");
    text.innerHTML='<br>'
    text.innerHTML+=' promedio general : '+funcion.promedio+'<br>';
    text.innerHTML+=' cantidad de retiros : '+funcion.retiros+"<br>";
    text.innerHTML+=' cantidad de consignaciones : '+funcion.consignaciones+"<br>";
    text.innerHTML+=' consignacion mayor : '+funcion.consMayor+"<br>";
    text.innerHTML+=' retiro menor : '+funcion.retiroMen+"<br>";
console.log(pilaCajero.historial());


}