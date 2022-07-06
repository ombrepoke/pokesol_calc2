<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Header</div>

                    <div class="card-body">
                        <form name="BscCalc">
                            <div id="Update" style="position:absolute;"></div>
                            <div id="Guide" style="position:absolute;"></div>
                            <input type="button" value="実行" v-on:click="ButtonON('End')" class="buttonA" style="width:48px;height:36px;"></input>
                            <input type="button" value="加算" v-on:click="ButtonON('Insert')" class="buttonA" style="width:48px;height:36px;"></input>
                            <br>
                            <br>
                            <span>威力</span>
                            <input type="text" name="MovePWR" value="100" style="width:48px;"></input>
                            <span>攻撃力</span>
                            <input type="text" name="UserPWR" value="100" style="width:48px;"></input>
                            <span>防御力</span>
                            <input type="text" name="FoeDEF" value="100" style="width:48px;"></input>
                            <span>一致</span>
                            <input type="text" name="TypeAMOD" value="1" style="width:48px;"></input>
                            <span>相性</span>
                            <input type="text" name="TypeBMOD" value="1" style="width:48px;"></input>
                            <span>HP</span>
                            <input type="text" name="MaxHP" onclick="return nextkeyChange(this)" onkeypress="return handleEnter(this, event)" style="width:48px;" tabindex="6"></input>
                            <span>ALv</span>
                            <input type="text" name="ALv" onclick="return nextkeyChange(this)" onkeypress="return handleEnter(this, event)" style="width:48px;" tabindex="-1"></input>
                            <span>DLv</span>
                            <input type="text" name="DLv" onclick="return nextkeyChange(this)" onkeypress="return handleEnter(this, event)" style="width:48px;" tabindex="-1"></input>
                            </form>
                        <br>
                        <table id="AddRes"></table>
                        <br>
                        <!-- <div id="KillRat"><table class="calc-res2"><tbody><tr><th style="width:23%;">急所</th><th style="width:23%;">2回合計</th><th style="width:27%;">5回合計</th><th style="width:27%;">前回との合算</th></tr><tr><td>2&nbsp;～&nbsp;3</td><td>2&nbsp;～&nbsp;4</td><td>5&nbsp;～&nbsp;10</td><td>1&nbsp;～&nbsp;2</td></tr></tbody></table></div> -->
                        <br>
                        <!-- <div id="CalcRes"><table class="calc-res2"><tbody><tr><th>威力</th><th>攻撃力</th><th>防御力</th><th>一致</th><th>相性</th><th>ダメージ</th></tr><tr><td class="sta">1</td><td class="sta">1</td><td class="sta">1</td><td class="sta">1</td><td class="sta">1</td><td class="dmg"> 1&nbsp;～&nbsp;2</td></tr><tr><th>威補正</th><th>攻補正</th><th>防補正</th><th>D補正</th><th colspan="2">火傷, 複数, 天候</th></tr><tr><td>1</td><td>1</td><td>1</td><td>1</td><td colspan="2">1,&nbsp;1,&nbsp;1</td></tr><tr><th colspan="3">攻撃力周囲値</th><th colspan="3">防御力周囲値</th></tr><tr><td colspan="3">&nbsp;</td><td colspan="3">&nbsp;</td></tr><tr><td colspan="3">&nbsp;</td><td colspan="3">&nbsp;</td></tr><tr class="nowcalc"><td colspan="3">NaN&nbsp;～&nbsp;2&nbsp;(&nbsp;1&nbsp;～&nbsp;2&nbsp;)</td><td colspan="3">&nbsp;</td></tr><tr><td colspan="3">3&nbsp;～&nbsp;4&nbsp;(&nbsp;2&nbsp;～&nbsp;3&nbsp;)</td><td colspan="3">&nbsp;</td></tr><tr><td colspan="3">5&nbsp;～&nbsp;6&nbsp;(&nbsp;3&nbsp;～&nbsp;4&nbsp;)</td><td colspan="3">&nbsp;</td></tr></tbody></table></div> -->
                        <br>
                        <div id="RndRes"><table class="calc-random"><tbody><tr><th>85</th><th>86</th><th>87</th><th>88</th><th>89</th><th>90</th><th>91</th><th>92</th><th>93</th><th>94</th><th>95</th><th>96</th><th>97</th><th>98</th><th>99</th><th>100</th></tr><tr><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>2</td></tr></tbody></table></div>                    </div>
                        <!-- <div id="MODRes"><div>複数対象</div><table class="calc-random"><tbody><tr><th>85</th><th>86</th><th>87</th><th>88</th><th>89</th><th>90</th><th>91</th><th>92</th><th>93</th><th>94</th><th>95</th><th>96</th><th>97</th><th>98</th><th>99</th><th>100</th></tr><tr><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td></tr></tbody></table></div> -->
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data () {
            return {
                ResHistory: [100],
                PokeReg: [100],
                nowkey: 0,
                nextkey: 0,
                tmpkey: 0,
                MaxHP: 0,
                hiskey: 0,
                nowhis: 0,
                add_vis: 0,
                expmod_vis: 1,
                calc_ver: '1.604',
                calc_memo: '2018/10/16',
                AddTbl: [],
                DmgTbl: [],
                DmgTblTmp: [],
                RevHis: 0,
                RevKey: 0,
                HisPage: 0,
            }
        },
        mounted() {
            console.log('Component mounted.')
        },
        methods: {
            ButtonON(sys){
                if( sys == 'Submit' ){
                } else if( sys == 'End' ){
                    this.DmgCalc('Bsc');
                    document.BscCalc.elements[0].focus();
                    document.BscCalc.elements[0].select();
                    this.nowkey=0;
                    this.tmpkey=0;
                    this.nextkey=5;
                    this.RevHis=0; 
                    this.RevKey=0;
                    document.getElementById('AddRes').style.visibility='hidden';
                    this.add_vis=1;
                    this.expmod_vis=1;
                    document.getElementById('Update').style.visibility='hidden';
                    document.getElementById('Guide').style.visibility='hidden';
                } else if( sys == 'Insert' ){
                    this.AddCalc(
                        this.ResHistory[this.RevHis][11],
                        this.ResHistory[this.RevHis][12],
                        this.ResHistory[this.RevHis][13],
                        this.ResHistory[this.RevHis][7],
                        this.ResHistory[this.RevHis][8],
                        this.ResHistory[this.RevHis][9],
                        this.ResHistory[this.RevHis][10]
                    );
                    document.getElementById('AddRes').style.visibility='visible'; 
                    this.add_vis=0;
                    document.getElementById('Update').style.visibility='hidden';
                    document.getElementById('Guide').style.visibility='hidden';
                    // document.getElementById('his' + this.RevHis).className='addhis';
                    // document.getElementById('his' + this.RevHis).backgroundColor='#f8d8c8';
                } else if( sys == 'Pause' ){
                } else if( sys == 'Esc' ){
                }
            },
            Asc(a,b){
                return (a[0]-b[0])
            },
            MWCalc(m,Mlt,Wea,PB){
                m = Math.floor( m / 50 ) + 2;
                m = Math.ceil( ( ( m * Mlt * 10 ) - 5 ) / 10 );
                m = Math.ceil( ( ( m * Wea * 10 ) - 5 ) / 10 );
                if( PB && PB != 99 ){ m = Math.floor( m * PB ); }
                if( !m || m < 0 ){ m=0; }
                return m;
            },
            MODCalc(m,A,B,Burn,Fin){
                m = Math.ceil( ( ( m * A * 10 ) - 5 ) / 10 );
                m = Math.floor( m * B );
                m = Math.floor( m / Burn );
                if ( m < 1 ){ m=1; };
                m = Math.ceil( ( ( m * Fin * 10 ) - 5 ) / 10 );
                return m;
            },
            RRT(MovePWR, UserPWR, DEF, Mlt, Wea, MoveMOD, UserMOD, TypeA, TypeB, Burn, Fin, PB, DEFMOD, ALv, DLv, SSMOD, DmgResPB, UserLv){
                if( ALv > 0 ) var UserPWRLv = Math.floor( UserPWR * ( 2 + ALv ) / 2 ); 
                else if ( ALv < 0 ) var UserPWRLv = Math.floor( UserPWR * 2 / ( 2 - ALv ) );
                else var UserPWRLv = UserPWR;

                if( DLv > 0 ) var DEFLv = Math.floor( Math.floor( DEF * SSMOD ) * ( 2 + DLv ) / 2 ); 
                else if ( DLv < 0 ) var DEFLv = Math.floor( Math.floor( DEF * SSMOD ) * 2 / ( 2 - DLv ) );
                else var DEFLv = Math.floor( DEF * SSMOD );

                var MovePWRRes = Math.ceil( ( ( MovePWR * MoveMOD * 10 ) - 5 ) / 10 );
                var UserPWRRes = Math.ceil( ( ( UserPWRLv * UserMOD * 10 ) - 5 ) / 10 );
                DEF = Math.ceil( ( ( DEFLv * DEFMOD * 10 ) - 5 ) / 10 );

                var Dmg = Math.floor( ( 2 * UserLv ) / 5 + 2 );
                Dmg = Math.floor( ( Dmg * MovePWRRes * UserPWRRes ) / DEF );
                Dmg = this.MWCalc(Dmg,Mlt,Wea,PB);

                var i;
                var RndResTbl='<table class="calc-random"><tr>';
                for (i=85; i <= 100; i++){
                    RndResTbl+='<th>' + i + '</th>';
                }
                RndResTbl+='</tr><tr>';
                for (i=0; i <= 15; i++){
                    var DmgTmp = Math.floor( ( Dmg * ( i + 85 ) ) / 100 );
                    var DmgTmpPB = Math.floor( ( DmgResPB * ( i + 85 ) ) / 100 );
                    if( !Dmg ){ DmgTmp = 0; }

                    DmgTmp = this.MODCalc(DmgTmp,TypeA,TypeB,Burn,Fin);
                    DmgTmpPB = this.MODCalc(DmgTmpPB,TypeA,TypeB,Burn,Fin);
                    if( PB == 99 ){ DmgTmp+=DmgTmpPB; }

                    RndResTbl+='<td>' + DmgTmp + '</td>';
                }
                RndResTbl+='</tr></table>';

                return RndResTbl;

            },
            ModExp(i,sys){
                var ResHisMod='';

                return ResHisMod;
            },
            DmgCalc(sys){
                this.RevHis=0; 
                this.RevKey=0;
                if( sys == 'Bsc' ){
                    var InputCalc = document.BscCalc;
                    var MoveMOD = 4096;
                    var UserMOD = 4096;
                    var FinMOD = 4096;
                    var PBMOD = 0;
                    var MovePWR = parseInt(InputCalc.MovePWR.value);
                    var UserPWR = parseInt(InputCalc.UserPWR.value);
                    var FoeDEF = parseInt(InputCalc.FoeDEF.value);
                    var DEFMOD = 4096;
                    var UserLv = 0;
                    var MaxHP = parseInt(InputCalc.MaxHP.value);
                    if( !InputCalc.TypeAMOD.value ) { var TypeAMOD = 1; } else { var TypeAMOD = parseFloat(InputCalc.TypeAMOD.value); }
                    if( !InputCalc.TypeBMOD.value ) { var TypeBMOD = 1; } else { var TypeBMOD = parseFloat(InputCalc.TypeBMOD.value); }
                    if( InputCalc.TypeAMOD.value.match(/[\/]/) == null ){
                        if( !InputCalc.TypeAMOD.value ) { var TypeAMOD = 1; } else { var TypeAMOD = parseFloat(InputCalc.TypeAMOD.value.replace( /[x*]/, '')); }
                    }else{ var TypeAMOD = 1 / parseFloat(InputCalc.TypeAMOD.value.replace( /\//, '')); }
                    if( InputCalc.TypeBMOD.value.match(/[\/]/) == null ){
                        if( !InputCalc.TypeBMOD.value ) { var TypeBMOD = 1; } else { var TypeBMOD = parseFloat(InputCalc.TypeBMOD.value.replace( /[x*]/, '')); }
                    }else{ var TypeBMOD = 1 / parseFloat(InputCalc.TypeBMOD.value.replace( /\//, '')); }

                    var MltMOD = 1;
                    var WeaMOD = 1;
                    var BurnMOD = 1;
                    var ALv = parseInt(InputCalc.ALv.value);
                    var DLv = parseInt(InputCalc.DLv.value);


                    var INPUT_HIS = [];
                        INPUT_HIS[0] = parseInt(InputCalc.MovePWR.value);
                        INPUT_HIS[1] = parseInt(InputCalc.UserPWR.value);
                        INPUT_HIS[2] = parseInt(InputCalc.FoeDEF.value);
                        INPUT_HIS[3] = parseFloat(InputCalc.TypeAMOD.value);
                        INPUT_HIS[4] = parseFloat(InputCalc.TypeBMOD.value);
                        INPUT_HIS[5] = parseInt(InputCalc.MaxHP.value);
                        INPUT_HIS[6] = parseInt(InputCalc.ALv.value);
                        INPUT_HIS[7] = parseInt(InputCalc.DLv.value);

                for (i=8; i <= 100; i++){
                    INPUT_HIS[i]=0;
                }

                    MoveMOD=MoveMOD / 4096;
                    UserMOD=UserMOD / 4096;
                    FinMOD=FinMOD / 4096;
                    DEFMOD=DEFMOD / 4096;

                } else if ( sys == 'Rel'){

                }

                if( !MovePWR ) MovePWR = 1;
                if( !UserPWR ) UserPWR = 1;
                if( !FoeDEF ) FoeDEF = 1;
                if( !MaxHP ) MaxHP = 0;
                if( TypeAMOD <= 0 ) TypeAMOD=1;
                if( TypeBMOD <= 0 ) TypeBMOD=1;
                if ( !ALv ) ALv=0;
                else if( ALv > 6 ) ALv=6;
                else if ( ALv < -6 ) ALv=-6;
                if( !DLv ) DLv=0;
                else if ( DLv > 6 ) DLv=6;
                else if ( DLv < -6 ) DLv=-6;
                var SSMOD=1;

                var DmgRes = 0;
                var UserLv = 50;
                if( UserLv == 0 ) var UserLv = parseInt(document.InputMode.ulv.value);
                if( !UserLv ) UserLv = 50;
                if( UserLv < 1 ) UserLv = 1;
                if( UserLv > 100 ) UserLv = 100;

                if( ALv > 0 ) var UserPWRLv = Math.floor( UserPWR * ( 2 + ALv ) / 2 ); 
                else if ( ALv < 0 ) var UserPWRLv = Math.floor( UserPWR * 2 / ( 2 - ALv ) );
                else var UserPWRLv = UserPWR;

                if( DLv > 0 ) var FoeDEFLv = Math.floor( Math.floor( FoeDEF * SSMOD ) * ( 2 + DLv ) / 2 ); 
                else if ( DLv < 0 ) var FoeDEFLv = Math.floor( Math.floor( FoeDEF * SSMOD ) * 2 / ( 2 - DLv ) );
                else var FoeDEFLv = Math.floor( FoeDEF * SSMOD );

                var MovePWRRes = Math.ceil( ( ( MovePWR * MoveMOD * 10 ) - 5 ) / 10 );
                var UserPWRRes = Math.ceil( ( ( UserPWRLv * UserMOD * 10 ) - 5 ) / 10 );
                var FoeDEFRes = Math.ceil( ( ( FoeDEFLv * DEFMOD * 10 ) - 5 ) / 10 );
                var DmgResPB = 0;

                DmgRes = Math.floor( ( 2 * UserLv ) / 5 + 2 );
                DmgRes = Math.floor( ( DmgRes * MovePWRRes * UserPWRRes ) / FoeDEFRes );
                if( PBMOD == 99 ) DmgResPB = this.MWCalc(DmgRes,MltMOD,WeaMOD,0.25);
                DmgRes = this.MWCalc(DmgRes,MltMOD,WeaMOD,PBMOD);

                var i = 0;
                var MaxDmgRes = 0;
                var MinDmgRes = 0;
                var CriMaxDmg = 0;
                var CriMinDmg = 0;
                var kp = 0;
                var wkp = 0;
                var kkp =0;
                var kpr = [
                    [0, 0],
                    [0, 0],
                    [0, 0, 0],
                    [0, 0],
                    [0, 0]
                ];
                var kkpr = [
                    [0, 0],
                    [0, 0],
                    [0, 0, 0],
                    [0, 0],
                    [0, 0]
                ];
                
                var RndResTbl='<table class="calc-random"><tr>';
                for (i=85; i <= 100; i++){
                    RndResTbl+='<th>' + i + '</th>';
                }
                RndResTbl+='</tr><tr>';
                for (i=0; i <= 15; i++){
                    var DmgTmp = Math.floor( ( DmgRes * ( 85 + i ) ) / 100 );
                    var DmgTmpPB = 0;
                    if( PBMOD == 99 ) DmgTmpPB = Math.floor( ( DmgResPB * ( 85 + i ) ) / 100 );
                    if( !DmgRes ){ DmgTmp = 0; }
                    DmgTmp = this.MODCalc(DmgTmp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                    DmgTmpPB = this.MODCalc(DmgTmpPB,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                    if( PBMOD == 99 ) DmgTmp = DmgTmp + DmgTmpPB;

                    if( MaxHP > 0 && MaxHP <= DmgTmp){ kp++; }

                    if( i == 0 ){ MinDmgRes = DmgTmp; }
                    if( i == 15 ){ MaxDmgRes = DmgTmp; }

                    if( MaxHP > MinDmgRes ){
                        for (j=0; j <= 15; j++){
                            var DmgTmpp = Math.floor( ( DmgRes * ( 85 + j ) ) / 100 );
                            var DmgTmppPB = 0;
                            if( PBMOD == 99 ) DmgTmppPB = Math.floor( ( DmgResPB * ( 85 + j ) ) / 100 );
                            if( !DmgRes ){ DmgTmpp = 0; }
                            DmgTmpp = this.MODCalc(DmgTmpp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                            DmgTmppPB = this.MODCalc(DmgTmppPB,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                            if( PBMOD == 99 ) DmgTmpp = DmgTmpp + DmgTmppPB;
                            if( ( DmgTmp + DmgTmpp ) >= MaxHP ){ wkp++; }
                        }
                        if( ( this.MODCalc(DmgRes,TypeAMOD,TypeBMOD,BurnMOD,FinMOD) + this.ResHistory[0][12] ) >= MaxHP ){
                            for (j=0; j <= 15; j++){
                                var DmgTmpp = Math.floor( ( this.ResHistory[0][13] * ( 85 + j ) ) / 100 );
                                var DmgTmppPB = 0;
                                if( this.ResHistory[0][14] == 'ON' ) DmgTmppPB = Math.floor( ( this.ResHistory[0][19] * ( 85 + j ) ) / 100 );
                                if( !DmgRes ){ DmgTmpp = 0; }
                                DmgTmpp = this.MODCalc(DmgTmpp,this.ResHistory[0][7],this.ResHistory[0][8],this.ResHistory[0][9],this.ResHistory[0][10]);
                                DmgTmppPB = this.MODCalc(DmgTmppPB,this.ResHistory[0][7],this.ResHistory[0][8],this.ResHistory[0][9],this.ResHistory[0][10]);
                                if( PBMOD == 99 ) DmgTmpp = DmgTmpp + DmgTmppPB;
                                if( ( DmgTmp + DmgTmpp ) >= MaxHP ){ kkp++; }
                            }
                        }
                    } else { wkp=256; }

                    RndResTbl+='<td>' + DmgTmp + '</td>';
                }
                RndRes.innerHTML=RndResTbl + '</tr></table>';
                if( PBMOD == 99 ) RndRes.innerHTML+='「おやこあい」は親と子で別々の乱数が発生します(16*16=256通り)。<br>上の乱数表は親と子で同じ乱数が発生した時の仮の値であり、単なる目安です。<br>「おやこあい」を正確に計算する場合は加算機能で「子」の補正を足して下さい。';

                // MODRes.innerHTML='<div align="left">複数対象</div>' + this.RRT(MovePWR,UserPWR,FoeDEF,0.75,WeaMOD,MoveMOD,UserMOD,TypeAMOD,TypeBMOD,BurnMOD,FinMOD,0,DEFMOD,ALv,DLv,SSMOD,DmgResPB,UserLv);

                if( MaxHP != 0 ){

                    if( kp != 0 ){
                        kpr[2][0]=kp;
                        for (i=1; !kpr[4][1]; i++){
                            var kp_tmp=0;
                            for (j=0; j<= 15; j++){
                                var DmgTmp = Math.floor( ( DmgRes * ( 85 + j ) ) / 100 );
                                if( !DmgRes ){ DmgTmp = 0; }
                                DmgTmp = this.MODCalc(DmgTmp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                                if( ( MaxHP + i ) <= DmgTmp ){ kp_tmp++; }
                            }
                            if( kpr[2][0] != kp_tmp && kpr[2][1] === 0 ){ kpr[2][1] = ( MaxHP + i - 1 ); kpr[3][0] = kp_tmp; }
                            else if ( kpr[2][0] >= 256 ){ kpr[2][2] = kpr[1][1] = kpr[0][1] = MaxHP; kpr[1][0] = kpr[0][0] = 256; break; }
                            if( kpr[3][0] != kp_tmp && kpr[3][1] === 0 && kpr[3][0] != 0 ){ kpr[3][1] = ( MaxHP + i - 1 ); kpr[4][0] = kp_tmp; }
                            else if ( kpr[2][0] == 0 && kpr[3][0] == 0 && kpr[2][1] != 0 ){ kpr[3][1] = kpr[4][1] = MaxHP; kpr[4][0] = 0; break; }
                            else if ( kpr[2][0] != 0 && kpr[3][0] == 0 && kpr[2][1] != 0 ){ kpr[3][1] = 999; kpr[4][1] = MaxHP; kpr[4][0] = 0; break; }
                            if( kpr[4][0] != kp_tmp && kpr[4][1] === 0 && kpr[4][0] != 0 ){ kpr[4][1] = ( MaxHP + i - 1 ); }
                            else if ( kpr[3][0] == 0 && kpr[4][0] == 0 && kpr[3][1] != 0 ){ kpr[4][1] = MaxHP; break; }
                            else if ( kpr[3][0] != 0 && kpr[4][0] == 0 && kpr[3][1] != 0 ){ kpr[4][1] = 999; break; }
                        }

                        for (i=1; !kpr[0][1]; i++){
                            var kp_tmp=0;
                            for (j=0; j<= 15; j++){
                                var DmgTmp = Math.floor( ( DmgRes * ( 85 + j ) ) / 100 );
                                if( !DmgRes ){ DmgTmp = 0; }
                                DmgTmp = this.MODCalc(DmgTmp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                                if( ( MaxHP - i ) <= DmgTmp ){ kp_tmp++; }
                            }
                            if( kpr[2][0] != kp_tmp && kpr[2][2] === 0 ){ kpr[2][2] = ( MaxHP - i + 1 ); kpr[1][0] = kp_tmp; }
                            else if ( kpr[2][0] == 16 ){ kpr[2][2] = kpr[1][1] = kpr[0][1] = MaxHP; kpr[1][0] = kpr[0][0] = 16; break;}
                            if( kpr[1][0] != kp_tmp && kpr[1][1] === 0 && kpr[2][2] != 0 ){ kpr[1][1] = ( MaxHP - i + 1 ); kpr[0][0] = kp_tmp; }
                            else if ( kpr[1][0] == 16 ){ kpr[1][1] = kpr[0][1] = MaxHP; kpr[0][0] = 16; break;}
                            if( kpr[0][0] != kp_tmp && kpr[0][1] === 0 && kpr[0][0] != 0 ){ kpr[0][1] = ( MaxHP - i + 1 ); }
                            else if ( kpr[0][0] == 16 ){ kpr[0][1] = MaxHP; break;}
                        }
                    } else if( kp == 0 && wkp != 0 ){
                        kpr[2][0]=wkp;
                        for (i=1; !kpr[4][1]; i++){
                            var kp_tmp=0;
                            for (j=0; j<= 15; j++){
                                var DmgTmp = Math.floor( ( DmgRes * ( 85 + j ) ) / 100 );
                                if( !DmgRes ){ DmgTmp = 0; }
                                DmgTmp = this.MODCalc(DmgTmp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                                for (k=0; k <= 15; k++){
                                    var DmgTmpp = Math.floor( ( DmgRes * ( 85 + k ) ) / 100 );
                                    if( !DmgRes ){ DmgTmpp = 0; }
                                    DmgTmpp = this.MODCalc(DmgTmpp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                                    if( ( DmgTmp + DmgTmpp ) >= ( MaxHP + i ) ){ kp_tmp++; }
                                }
                            }
                            if( kpr[2][0] != kp_tmp && kpr[2][1] === 0 ){ kpr[2][1] = ( MaxHP + i - 1 ); kpr[3][0] = kp_tmp; }
                            else if ( kpr[2][0] == 0 ){ kpr[3][1] = kpr[4][1] = MaxHP; break; }

                            if( kpr[3][0] != kp_tmp && kpr[3][1] === 0 && kpr[3][0] != 0 ){ kpr[3][1] = ( MaxHP + i - 1 ); kpr[4][0] = kp_tmp; }
                            else if ( kpr[2][0] == 0 && kpr[3][0] == 0 && kpr[2][1] != 0 ){ kpr[3][1] = kpr[4][1] = MaxHP; kpr[4][0] = 0; break; }
                            else if ( kpr[2][0] != 0 && kpr[3][0] == 0 && kpr[2][1] != 0 ){ kpr[3][1] = 999; kpr[4][1] = MaxHP; kpr[4][0] = 0; break; }

                            if( kpr[4][0] != kp_tmp && kpr[4][1] === 0 && kpr[4][0] != 0 ){ kpr[4][1] = ( MaxHP + i - 1 ); }
                            else if ( kpr[3][0] == 0 && kpr[4][0] == 0 && kpr[3][1] != 0 ){ kpr[4][1] = MaxHP; break; }
                            else if ( kpr[3][0] != 0 && kpr[4][0] == 0 && kpr[3][1] != 0 ){ kpr[4][1] = 999; break; }
                        }

                        for (i=1; !kpr[0][1]; i++){
                            var kp_tmp=0;
                            for (j=0; j<= 15; j++){
                                var DmgTmp = Math.floor( ( DmgRes * ( 85 + j ) ) / 100 );
                                if( !DmgRes ){ DmgTmp = 0; }
                                DmgTmp = this.MODCalc(DmgTmp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                                for (k=0; k <= 15; k++){
                                    var DmgTmpp = Math.floor( ( DmgRes * ( 85 + k ) ) / 100 );
                                    if( !DmgRes ){ DmgTmpp = 0; }
                                    DmgTmpp = this.MODCalc(DmgTmpp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                                    if( ( DmgTmp + DmgTmpp ) >= ( MaxHP - i ) ){ kp_tmp++; }
                                }
                            }
                            if( kpr[2][0] != kp_tmp && kpr[2][2] === 0 ){ kpr[2][2] = ( MaxHP - i + 1 ); kpr[1][0] = kp_tmp; }
                            else if ( kpr[2][0] >= 256 ){ kpr[2][2] = kpr[1][1] = kpr[0][1] = MaxHP; kpr[1][0] = kpr[0][0] = 256; break; }
                            if( kpr[1][0] != kp_tmp && kpr[1][1] === 0 && kpr[2][2] != 0 ){ kpr[1][1] = ( MaxHP - i + 1 ); kpr[0][0] = kp_tmp; }
                            else if ( kpr[1][0] >= 256 ){ kpr[1][1] = kpr[0][1] = MaxHP; kpr[0][0] = 256; break; }
                            if( kpr[0][0] != kp_tmp && kpr[0][1] === 0 && kpr[0][0] != 0 ){ kpr[0][1] = ( MaxHP - i + 1 ); }
                            else if ( kpr[0][0] >= 256 ){ kpr[0][1] = MaxHP; break; }
                        }
                    }
                    if( kkp != 0 ){
                        kkpr[2][0]=kkp;
                        for (i=1; !kkpr[4][1]; i++){
                            var kp_tmp=0;
                            for (j=0; j<= 15; j++){
                                var DmgTmp = Math.floor( ( DmgRes * ( 85 + j ) ) / 100 );
                                if( !DmgRes ){ DmgTmp = 0; }
                                DmgTmp = this.MODCalc(DmgTmp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                                for (k=0; k <= 15; k++){
                                    var DmgTmpp = Math.floor( ( this.ResHistory[0][13] * ( 85 + k ) ) / 100 );
                                    if( !DmgRes ){ DmgTmpp = 0; }
                                    DmgTmpp = this.MODCalc(DmgTmpp,this.ResHistory[0][7],this.ResHistory[0][8],this.ResHistory[0][9],this.ResHistory[0][10]);
                                    if( ( DmgTmp + DmgTmpp ) >= ( MaxHP + i ) ){ kp_tmp++; }
                                }
                            }
                            if( kkpr[2][0] != kp_tmp && kkpr[2][1] === 0 ){ kkpr[2][1] = ( MaxHP + i - 1 ); kkpr[3][0] = kp_tmp; }
                            else if ( kkpr[2][0] == 0 ){ kkpr[3][1] = kkpr[4][1] = MaxHP; kkpr[3][0] = kkpr[4][0] = 0; break; }

                            if( kkpr[3][0] != kp_tmp && kkpr[3][1] === 0 && kkpr[3][0] != 0 ){ kkpr[3][1] = ( MaxHP + i - 1 ); kkpr[4][0] = kp_tmp; }
                            else if ( kkpr[2][0] == 0 && kkpr[3][0] == 0 && kkpr[2][1] != 0 ){ kkpr[3][1] = kkpr[4][1] = MaxHP; kkpr[4][0] = 0; break; }
                            else if ( kkpr[2][0] != 0 && kkpr[3][0] == 0 && kkpr[2][1] != 0 ){ kkpr[3][1] = 999; kkpr[4][1] = MaxHP; kkpr[4][0] = 0; break; }

                            if( kkpr[4][0] != kp_tmp && kkpr[4][1] === 0 && kkpr[4][0] != 0 ){ kkpr[4][1] = ( MaxHP + i - 1 ); break; }
                            else if ( kkpr[3][0] == 0 && kkpr[4][0] == 0 && kkpr[3][1] != 0 ){ kkpr[4][1] = MaxHP; break; }
                            else if ( kkpr[3][0] != 0 && kkpr[4][0] == 0 && kkpr[3][1] != 0 ){ kkpr[4][1] = 999; break; }
                        }

                        for (i=1; !kkpr[0][1]; i++){
                            var kp_tmp=0;
                            for (j=0; j<= 15; j++){
                                var DmgTmp = Math.floor( ( DmgRes * ( 85 + j ) ) / 100 );
                                if( !DmgRes ){ DmgTmp = 0; }
                                DmgTmp = this.MODCalc(DmgTmp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                                for (k=0; k <= 15; k++){
                                    var DmgTmpp = Math.floor( ( this.ResHistory[0][13] * ( 85 + k ) ) / 100 );
                                    if( !DmgRes ){ DmgTmpp = 0; }
                                    DmgTmpp = this.MODCalc(DmgTmpp,this.ResHistory[0][7],this.ResHistory[0][8],this.ResHistory[0][9],this.ResHistory[0][10]);
                                    if( ( DmgTmp + DmgTmpp ) >= ( MaxHP - i ) ){ kp_tmp++; }
                                }
                            }
                            if( kkpr[2][0] != kp_tmp && kkpr[2][2] === 0 ){ kkpr[2][2] = ( MaxHP - i + 1 ); kkpr[1][0] = kp_tmp; }
                            else if ( kkpr[2][0] >= 256 ){ kkpr[2][2] = kkpr[1][1] = kkpr[0][1] = MaxHP; kkpr[1][0] = kkpr[0][0] = 256; break; }
                            if( kkpr[1][0] != kp_tmp && kkpr[1][1] === 0 && kkpr[2][2] != 0 ){ kkpr[1][1] = ( MaxHP - i + 1 ); kkpr[0][0] = kp_tmp; }
                            else if ( kkpr[1][0] >= 256 ){ kkpr[1][1] = 0; kkpr[0][1] = MaxHP; kkpr[0][0] = 256; break; }
                            if( kkpr[0][0] != kp_tmp && kkpr[0][1] === 0 && kkpr[0][0] != 0 ){ kkpr[0][1] = ( MaxHP - i + 1 ); }
                            else if ( kkpr[0][0] >= 256 ){ kkpr[0][1] = 0; kkpr[0][0] = 256; break; }
                        }
                    }

                    var KillRatTbl='<table class="calc-res"><tr><th colspan="3">HPに対する割合</th></tr>';
                    KillRatTbl+=
                    '<tr><td colspan="3">' + ( Math.floor( ( MinDmgRes / MaxHP ) * 1000 ) / 10 ) + '%&nbsp;～&nbsp;' + ( Math.floor( ( MaxDmgRes / MaxHP ) * 1000 ) / 10 ) + '%</td></tr>'
                    + '<tr><th>2回合計</th><th>5回合計</th><th>前回との合算</th></tr>'
                    + '<tr><td>' + ( MinDmgRes * 2 ) + '&nbsp;～&nbsp;' + ( MaxDmgRes * 2 )
                    + '</td><td>' + ( MinDmgRes * 5 ) + '&nbsp;～&nbsp;' + ( MaxDmgRes * 5 ) + '</td>';
                    if( this.ResHistory[0][11] > 2 ){
                        KillRatTbl+='<td>' + ( MinDmgRes + this.ResHistory[0][11]) + '&nbsp;～&nbsp;' + ( MaxDmgRes + this.ResHistory[0][12] ) + '</td></tr>';
                    } else {
                        KillRatTbl+='<td>&nbsp;</td></tr>';
                    }

                    KillRatTbl+='<tr><th>撃破率(1回)</th><th>撃破率(2回)</th><th>合算確率</th></tr>';
                    KillRatTbl+=
                    '<tr><td>' + kp + '/16&nbsp;(' + ( ( kp / 16 ) * 100 ) + '%)</td>'
                    + '<td>' + wkp + '/256&nbsp;(' + Math.floor( ( wkp / 256 ) * 10000 ) / 100 + '%)</td>'
                    + '<td>' + kkp + '/256&nbsp;(' + Math.floor( ( kkp / 256 ) * 10000 ) / 100 + '%)</td></tr>';

                    if( kpr[2][0] != 0 || kkpr[2][0] != 0 ){

                        if( kp != 0 ){ KillRatTbl+='<tr><th>HP周囲値(1回)</th><th colspan="2">'; var frac=16; }
                        else{ KillRatTbl+='<tr><th>HP周囲値(2回)</th><th colspan="2">'; var frac=256; }

                        if( kkp != 0 ) KillRatTbl+='HP周囲値(合算)';
                        else KillRatTbl+='---';

                        KillRatTbl+='</th></tr>';

                        if ( kpr[0][1] != MaxHP ) KillRatTbl+='<tr><td>' + kpr[0][1] + '&nbsp;～&nbsp;' + ( kpr[1][1] - 1 ) + '&nbsp;(&nbsp;' + kpr[0][0] + '/' + frac + '&nbsp;)</td><td colspan="2">';
                        else KillRatTbl+='<tr><td>---</td><td colspan="2">';

                        if ( kkpr[0][1] != MaxHP && kkp != 0 ) KillRatTbl+=kkpr[0][1] + '&nbsp;～&nbsp;' + ( kkpr[1][1] - 1 ) + '&nbsp;(&nbsp;' + kkpr[0][0] + '/256&nbsp;)';
                        else KillRatTbl+='---';
                        KillRatTbl+='</td></tr>';


                        if ( kpr[1][1] != MaxHP ) KillRatTbl+='<tr><td>' + kpr[1][1] + '&nbsp;～&nbsp;' + ( kpr[2][2] - 1 ) + '&nbsp;(&nbsp;' + kpr[1][0] + '/' + frac + '&nbsp;)</td><td colspan="2">';
                        else KillRatTbl+='<tr><td>---</td><td colspan="2">';

                        if ( kkpr[1][1] != MaxHP && kkp != 0  ) KillRatTbl+=kkpr[1][1] + '&nbsp;～&nbsp;' + ( kkpr[2][2] - 1 ) + '&nbsp;(&nbsp;' + kkpr[1][0] + '/256&nbsp;)';
                        else KillRatTbl+='---';
                        KillRatTbl+='</td></tr>';

                        KillRatTbl+='<tr class="nowcalc"><td>' + kpr[2][2] + '&nbsp;～&nbsp;' + kpr[2][1] + '&nbsp;(&nbsp;' + kpr[2][0] + '/' + frac + '&nbsp;)</td><td colspan="2">';

                        if ( kkp != 0 ) KillRatTbl+=kkpr[2][2] + '&nbsp;～&nbsp;' + kkpr[2][1] + '&nbsp;(&nbsp;' + kkpr[2][0] + '/256&nbsp;)';
                        else KillRatTbl+='---';
                        KillRatTbl+='</td></tr>';

                        if ( kpr[3][1] != MaxHP ) KillRatTbl+='<tr><td>' + ( kpr[2][1] + 1 ) + '&nbsp;～&nbsp;' + kpr[3][1] + '&nbsp;(&nbsp;' + kpr[3][0] + '/' + frac + '&nbsp;)</td><td colspan="2">';
                        else KillRatTbl+='<tr><td>---</td><td colspan="2">';

                        if ( kkpr[3][1] != MaxHP && kkp != 0  ) KillRatTbl+=( kkpr[2][1] + 1 ) + '&nbsp;～&nbsp;' + kkpr[3][1] + '&nbsp;(&nbsp;' + kkpr[3][0] + '/256&nbsp;)';
                        else KillRatTbl+='---';
                        KillRatTbl+='</td></tr>';

                        if ( kpr[4][1] != MaxHP ) KillRatTbl+='<tr><td>' + ( kpr[3][1] + 1 ) + '&nbsp;～&nbsp;' + kpr[4][1] + '&nbsp;(&nbsp;' + kpr[4][0] + '/' + frac + '&nbsp;)</td><td colspan="2">';
                        else KillRatTbl+='<tr><td>---</td><td colspan="2">';

                        if ( kkpr[4][1] != MaxHP && kkp != 0  ) KillRatTbl+=( kkpr[3][1] + 1 ) + '&nbsp;～&nbsp;' + kkpr[4][1] + '&nbsp;(&nbsp;' + kkpr[4][0] + '/256&nbsp;)';
                        else KillRatTbl+='---';
                        KillRatTbl+='</td></tr>';
                    }

                    KillRatTbl+='</table>';
                    KillRat.innerHTML=KillRatTbl;
                } else {
                    var KillRatTbl='<table class="calc-res2"><tr><th style="width:23%;">急所</th><th style="width:23%;">2回合計</th><th style="width:27%;">5回合計</th><th style="width:27%;">前回との合算</th></tr>';
                    var CriPWR = UserPWRRes;
                    var CriDEF = FoeDEFRes;
                    if( ALv < 0 ) var CriPWR = Math.ceil( ( ( UserPWR * MoveMOD * 10 ) - 5 ) / 10 );
                    if ( DLv > 0 ) var CriDEF = Math.ceil( ( ( Math.floor( FoeDEF * SSMOD ) * DEFMOD * 10 ) - 5 ) / 10 );;
                    KillRatTbl+=
                    '<tr><td>' + this.MODCalc(Math.floor( this.MWCalc(Math.floor( ( Math.floor( ( 2 * UserLv ) / 5 + 2 ) * MovePWRRes * CriPWR ) / CriDEF ),MltMOD,WeaMOD,1.5) * 0.85 ),TypeAMOD,TypeBMOD,BurnMOD,FinMOD) + '&nbsp;～&nbsp;' + this.MODCalc(this.MWCalc(Math.floor( ( Math.floor( ( 2 * UserLv ) / 5 + 2 ) * MovePWRRes * CriPWR ) / CriDEF ),MltMOD,WeaMOD,1.5),TypeAMOD,TypeBMOD,BurnMOD,FinMOD)
                    + '</td><td>' + ( MinDmgRes * 2 ) + '&nbsp;～&nbsp;' + ( MaxDmgRes * 2 )
                    + '</td><td>' + ( MinDmgRes * 5 ) + '&nbsp;～&nbsp;' + ( MaxDmgRes * 5 );
                    if( this.ResHistory[0] ){
                        KillRatTbl+='</td><td>' + ( MinDmgRes + this.ResHistory[0][11]) + '&nbsp;～&nbsp;' + ( MaxDmgRes + this.ResHistory[0][12] ) + '</td></tr>';
                    } else {
                        KillRatTbl+='</td><td>' + MinDmgRes + '&nbsp;～&nbsp;' + MaxDmgRes + '</td></tr>';
                    }
                    KillRatTbl+='</table>';
                    // KillRat.innerHTML=KillRatTbl;
                }

                // CallCalcRes(MovePWR, UserPWR, FoeDEF, MltMOD, WeaMOD, MoveMOD, UserMOD, TypeAMOD, TypeBMOD, BurnMOD, FinMOD, MinDmgRes, MaxDmgRes, DmgRes, PBMOD, DEFMOD, ALv, DLv, SSMOD, DmgResPB, UserLv);

                var His = 50;
                // if( !His || His == 10 || His == 15 ){ document.cookie = 'His=50; max-age=157680000'; His=50; }
                if( sys != 'Rel' ){
                    for (i=His+1; i > 0; i--){
                        this.ResHistory[i]=this.ResHistory[i-1];
                    }
                    var AurMOD = 0;
                    var SpoMOD = 0;
                    var FldMOD = 0;
                    var FlgMOD = 0;

                    this.ResHistory[0] = [MovePWR, UserPWR, FoeDEF, MltMOD, WeaMOD, MoveMOD, UserMOD, TypeAMOD, TypeBMOD, BurnMOD, FinMOD, MinDmgRes, MaxDmgRes, DmgRes, PBMOD, DEFMOD, ALv, DLv, SSMOD, DmgResPB, AurMOD, SpoMOD, FldMOD, FlgMOD, UserLv, INPUT_HIS];
                }
                /*
                    0 ... MovePWR
                    1 ... UserPWR
                    2 ... FoeDEF
                    ...
                    23 ... FlgMOD
                    24 ... UserLv
                    25 ... INPUT_HIS
                */

                var HisPage=0;
                var ResHisTbl='<table class="calc-res" style="clear: both;"><tr><th colspan="7">計算履歴</th></tr>';
                for (i=0; this.ResHistory[ i + 1 ] && i < 10; i++){
                        ResHisTbl+='<tr id="his' + i + '" class="onreshis" onmouseover="this.style.backgroundColor=\'#e8d8f8\';CallRndRes(' + i + ')" onclick="this.style.backgroundColor=\'#c8d8f8\';this.className=\'clickhis\';this.onmouseout=\'\';OnClickChange(' + i + ');CallInputHis(' + i + ');onmouseover=\'\'" onmouseout="this.style.backgroundColor=\'#ffffff\';HistoryRefresh(0)"><td class="sta2">' + this.ResHistory[i][0] + '</td><td class="sta2">' + this.ResHistory[i][1] + '</td><td class="sta2">' + this.ResHistory[i][2] + '</td><td class="sta2">' + this.ResHistory[i][7] + '</td><td class="sta2">' + this.ResHistory[i][8] + '</td><td class="others">';
                    var ResHisMod=this.ModExp(i,0);
                    ResHisTbl+= ResHisMod + '</td><td class="dmg"> ' + this.ResHistory[i][11] + '&nbsp;～&nbsp;' + this.ResHistory[i][12] + '</td></tr>';
                }
                for (i; i < 10; i++){
                    ResHisTbl+='<tr><td class="sta2">&nbsp;</td><td class="sta2">&nbsp;</td><td class="sta2">&nbsp;</td><td class="sta2">&nbsp;</td><td class="sta2">&nbsp;</td><td class="others">&nbsp;</td><td class="dmg">&nbsp;</td></tr>';
                }
                ResHisTbl+='<tr><td colspan="7" align="center">';
                for(i=0; i < ( His / 10 ); i++){
                if( i == 0 ) ResHisTbl+='<span style="background-color:#e8d8f8;">&nbsp;&nbsp;' + ( i + 1 ) + '&nbsp;&nbsp;</span>';
                else ResHisTbl+='<a href="Javascript: void(0)" onclick="HistoryRefresh(' + i + ')">&nbsp;&nbsp;' + ( i + 1 ) + '&nbsp;&nbsp;</a>';
                }
                ResHisTbl+='</td></tr></table>';
                // ResHis.innerHTML=ResHisTbl;

            },
            AddCalc(MinDmgRes,MaxDmgRes,DmgRes,TypeAMOD,TypeBMOD,BurnMOD,FinMOD) {

                var DmgTblTmpp=[];
                var j=0;
                var rnd=0;
                for(var i=0; this.AddTbl[i]; i++){}
                this.AddTbl[i]=[ MinDmgRes, MaxDmgRes ];

                if( !this.DmgTblTmp[0] ){
                    for (i=0; i <= 15; i++){
                        var DmgTmp = Math.floor( ( DmgRes * ( 85 + i ) ) / 100 );
                        DmgTmp = this.MODCalc(DmgTmp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                        if( i > 0 ){
                            if( this.DmgTblTmp[j-1][0] == DmgTmp ){ this.DmgTblTmp[j-1][1]++; }
                            else{ this.DmgTblTmp[j]=[ DmgTmp, 1 ]; j++; }
                        } else{ this.DmgTblTmp[j]=[ DmgTmp, 1 ]; j++; }
                    }
                    if( !this.DmgTbl[0] ) this.DmgTbl=this.DmgTblTmp;
                    rnd=16;
                } else {
                    for (i=0; i <= 15; i++){
                        var DmgTmp = Math.floor( ( DmgRes * ( 85 + i ) ) / 100 );
                        DmgTmp = this.MODCalc(DmgTmp,TypeAMOD,TypeBMOD,BurnMOD,FinMOD);
                        if( i > 0 ){
                            if( DmgTblTmpp[j-1][0] == DmgTmp ){ DmgTblTmpp[j-1][1]++; }
                            else{ DmgTblTmpp[j]=[ DmgTmp, 1 ]; j++; }
                        } else{ DmgTblTmpp[j]=[ DmgTmp, 1 ]; j++; }
                    }
                }
                if( DmgTblTmpp[0] ){
                    var k=0;
                    this.DmgTbl=[];
                    rnd=0;
                    for (i=0; this.DmgTblTmp[i]; i++){
                        for(j=0; DmgTblTmpp[j]; j++){
                            var DmgTmp = this.DmgTblTmp[i][0] + DmgTblTmpp[j][0];
                            if( j > 0 ){
                                if( this.DmgTbl[k-1][0] == DmgTmp ){ this.DmgTbl[k-1][1]+=this.DmgTblTmp[i][1]*DmgTblTmpp[j][1]; }
                                else{ this.DmgTbl[k]=[ DmgTmp, this.DmgTblTmp[i][1]*DmgTblTmpp[j][1] ]; k++; }
                            } else{ this.DmgTbl[k]=[ DmgTmp, this.DmgTblTmp[i][1]*DmgTblTmpp[j][1] ]; k++; }
                        }
                    }

                    this.DmgTbl.sort(this.Asc);
                    k=0;
                    for (i=0; this.DmgTbl[i]; i++){
                        rnd+=this.DmgTbl[i][1];
                        if( this.DmgTbl[i-1] ){
                            if( this.DmgTbl[i][0] == this.DmgTbl[i-1][0] ){
                                this.DmgTbl[i-1][1]+=this.DmgTbl[i][1];
                                this.DmgTbl.splice(i,1);
                                i--;
                            }
                        }
                    }

                    this.DmgTblTmp=this.DmgTbl;
                    DmgTblTmpp=[];
                }

                var td_width = 104;
                var td_width_b = 16;

                if(document.all){
                    var AddResTbl='<table class="calc-others" width="485px;"><tr><th class="others" colspan="8">加算ダメージ</th></tr>';
                } else {
                    var AddResTbl='<table class="calc-others"><tr><th class="others" colspan="8">加算ダメージ</th></tr>';
                }

                var AddTbl_a='<tr>';
                var AddTbl_b='<tr>';

                for(i=0; i < 8; i++){
                    if( ( i % 2 ) == 0 && this.AddTbl[i] ) AddTbl_a+='<td class="others" style="width:' + td_width_b + 'px;">' + ( i + 1 ) + '</td><td class="others" style="width:' + td_width + 'px;text-align:center">' + this.AddTbl[i][0] + '&nbsp;～&nbsp;' + this.AddTbl[i][1] + '</td>';
                    else if( ( i % 2 ) == 0 ) AddTbl_a+='<td class="others" style="width:' + td_width_b + 'px;">' + ( i + 1 ) + '</td><td class="others" style="width:' + td_width + 'px;text-align:center">&nbsp;</td>';
                    else if( this.AddTbl[i] ) AddTbl_b+='<td class="others" style="width:' + td_width_b + 'px;">' + ( i + 1 ) + '</td><td class="others" style="width:' + td_width + 'px;text-align:center">' + this.AddTbl[i][0] + '&nbsp;～&nbsp;' + this.AddTbl[i][1] + '</td>';
                    else AddTbl_b+='<td class="others" style="width:' + td_width_b + 'px;">' + ( i + 1 ) + '</td><td class="others" style="width:' + td_width + 'px;text-align:center">&nbsp;</td>';
                }
                AddTbl_a+='</tr>';
                AddTbl_b+='</tr>';
                AddResTbl+= AddTbl_a + AddTbl_b;
                //AddResTbl+='<tr><td class="others" colspan="8">' + rnd + '分率</td></tr>';
                AddResTbl+='<tr><th class="others" colspan="8">合計</th></tr>';
                var Total=0;
                var AddTblTmp = [];
                for (i=0; i < 80; i++){
                    var j = ( i % 20 );
                    if( !AddTblTmp[j] ) AddTblTmp[j]='<tr>';
                    if( this.DmgTbl[i] ){
                        AddTblTmp[j]+='<td class="others" style="width:' + ( td_width + td_width_b ) + 'px;text-align:left;" colspan="2">' + this.DmgTbl[i][0] + '(' + ( Math.floor( ( this.DmgTbl[i][1] / rnd ) * 10000 ) /100 )  + '%/' + ( Math.floor( ( ( 100 - ( ( Total / rnd ) * 10000 ) /100 ) ) * 100 ) / 100 )  + '%)</td>';
                        Total+=this.DmgTbl[i][1];
                    }
                    else AddTblTmp[j]+='<td class="others" style="width:' + ( td_width + td_width_b ) + 'px;text-align:left;" colspan="2">&nbsp;</td>';
                    if( i >= 60 ) AddTblTmp[j]+='</tr>';
                }
                for (i=0; i < 20; i++){
                    AddResTbl+=AddTblTmp[i];
                }
                //AddResTbl+=AddTblTmp;
                AddResTbl+='</table>';
                AddRes.innerHTML=AddResTbl;
            }
        }
    }


</script>
