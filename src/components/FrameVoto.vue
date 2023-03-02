<template>
  <div class="box">
    <div class="game_form" :style="css_form">
        <div class="game_data">
            <p class="data_theme">Select Theme</p>
            <div class="join_button">       
                <button :class="theme.class_th" v-for="(theme, index) in themes" :key="theme.id" @click="select_theme(index)">{{ theme.th }}</button>
            </div>
            <p class="data_theme">Numbers Players</p>
            <div class="join_button">
                <button :class="player.class_pl" v-for="(player, index) in players" :key="player.id" @click="number_players(index)">{{ player.pl }}</button>
            </div>
            <p class="data_theme">Grid Size</p>
            <div class="join_button">
                <button :class="grid.class_gr" v-for="(grid, index) in grids" :key="grid.id" @click="grid_size(index)">{{ grid.gr }} x {{ grid.gr }}</button>
            </div>
            <button class="send_game" @click="begin_game">Start Game</button>
        </div>
    </div>
    <!-- PLATAFORMA DEL JUEGO -->
    <div class="frame_game" :style="css_frame_game">
        <div class="inbox">
            <div class="side_left">memory</div>
            <div class="side_right">
                <button class="button_game" type="button" @click="restart_game">Restart</button>
                <button class="button_game" type="button" @click="new_game">New Game</button>
            </div>
        </div>
        <div class="game_box" :style="css_number">
            <button :class="group.class_circle" v-for="(group, index) in group_value" :key="group.id" @click="capture(index)" :disabled="group.stop_circle">{{ group.item }}</button>
        </div>
        <div class="another_box" v-if="setup_players==1">
            <div class="another_box_player">
                <div class="game_tag">Time</div>
                <div class="deposit">{{ time_begin }}</div>
            </div>
            <div class="another_box_player">
                <div class="game_tag">Move</div>
                <div class="deposit">{{ move_player }}</div>
            </div>
        </div>
        <div class="another_box" v-else>
            <div :class="point.css_select" v-for="point in point_player" :key="point.id">
                <div class="game_tag">Players {{ point.name_player }}</div>
                <div class="deposit">{{ point.point_game }}</div>
            </div>
        </div>
    </div>
    <!-- FIN PLATAFORMA DEL JUEGO -->
    <div class="result_game" :style="css_result_game">
        <div class="result_personal">
            <p class="data_result">You Did it!</p>
            <p class="data_result_second">Game Over! Here's how you got on...</p>
            <div class="another_box" v-if="setup_players==1">
                <div class="another_box_player">
                    <div class="game_tag">Time Elapsed</div>
                    <div class="result_tag">{{ time_begin }}</div>
                </div>
                <div class="another_box_player">
                    <div class="game_tag">Moves Taken</div>
                    <div class="result_tag">{{ move_player }} Moves</div>
                </div>
            </div>
            <div class="another_box" v-else>
                <div class="another_box_player" v-for="point in point_player" :key="point.id">
                    <div class="game_tag">Players {{ point.name_player }} <span v-if="value_winner==point.point_game">(Winner)</span></div>
                    <div class="result_tag">{{ point.point_game }} Pairs</div>
                </div>
            </div>
            <button class="send_game" type="button" @click="restart_game">Restart</button>
            <button class="send_game" type="button" @click="new_game">Setup New Game</button>
        </div>
    </div>
  </div>
</template>
<script>
export default {
    data() {
        return{
            css_number:"",
            css_form:"",
            css_frame_game:{"display":"none"},
            css_result_game:{"visibility":"hidden"},
            setup_themes:"",
            setup_players:"",
            setup_grids:0,
            put_grid:0,
            cap_number:[], // matriz de las variables elegidas
            circle_index:[],
            save_index:[], // matriz de las variables seleccionadas
            point_player:[],
            finish_game:[],
            turn_up:0,
            move_player:0,
            time_begin:"0:00",
            current_time:"",
            group_value:[],
            value_winner:"",
            themes:[
                {th:"Numbers",class_th:"data_button"},
                {th:"Icons",class_th:"data_button"},
            ],
            players:[
                {pl:1,class_pl:"data_button"},
                {pl:2,class_pl:"data_button"},
                {pl:3,class_pl:"data_button"},
                {pl:4,class_pl:"data_button"},
            ],
            grids:[
                {gr:4,class_gr:"data_button",style_grid:{"grid-template-columns":"70px 70px 70px 70px","grid-template-rows":"70px 70px 70px 70px"}},
                {gr:6,class_gr:"data_button",style_grid:{"grid-template-columns":"70px 70px 70px 70px 70px 70px","grid-template-rows":"70px 70px 70px 70px 70px 70px"}},
            ],
            
        };
    },
    methods: {
        select_theme(index){
            this.themes.map(function (x){
                x.class_th="data_button";
            });
            this.themes[index].class_th="data_button_push";
            this.setup_themes=this.themes[index].th;
        },
        number_players(index){
            
            this.players.map(function (x){
                x.class_pl="data_button";
            });
            
            this.players[index].class_pl="data_button_push";
            this.setup_players=this.players[index].pl;
        },
        grid_size(index){
            
            this.grids.map(function (x){
                x.class_gr="data_button";
            });
            this.grids[index].class_gr="data_button_push";
            this.setup_grids=this.grids[index].gr;
            this.css_number=this.grids[index].style_grid;
        },
        new_game(){
            this.css_result_game={"visibility":"hidden"};
            this.css_frame_game={"display":"none"};

            if(this.setup_players==1){
            clearInterval(this.current_time);
            }
            this.css_form="";
            this.setup_themes="";
            this.setup_players="";
            this.put_grid=0;
            this.cap_number=[]; // matriz de las variables elegidas
            this.circle_index=[];
            this.save_index=[]; // matriz de las variables seleccionadas
            this.move_player=0;
            this.time_begin="0:00";
            this.current_time="";
            this.group_value=[];
            this.point_player=[];
            this.finish_game=[];
            this.turn_up=0;
            this.value_winner="";
        },
        restart_game(){
            this.css_result_game={"visibility":"hidden"};
            this.time_begin="0:00";
            this.move_player=0;
            this.cap_number=[];
            this.circle_index=[];
            this.save_index=[];
            this.group_value=[];
            this.point_player=[];
            this.finish_game=[];
            this.value_winner="";
            this.turn_up=0;
    
            if(this.setup_players==1){
                clearInterval(this.current_time);
            }
            this.begin_game();
        },
        begin_game(){
            this.css_form={"display":"none"};
            this.css_frame_game={"display":"block"};
            var power = Math.pow(this.setup_grids,2)/2;
            var array_number=[];
            var follow=true;
            
            for(let i=0; i < power; i++){
                while( follow==true ){
                    var circle = Math.floor(Math.random()*100)+1;
                    if( !array_number.includes(circle) ){
                        array_number.push(circle);
                        follow=false;
                    }
                }
                follow=true;
            }

            console.log(power);

            var general = [];
            // se genera los primeros numeros aleatorios
            array_number.map(function(x) { general.push({item:x,stop_circle:false,class_circle:"style_cell",class_icon:x+".png"}); });
            // se duplica los numeros aleatorios
            array_number.map(function(x) { general.push({item:x,stop_circle:false,class_circle:"style_cell",class_icon:x+".png"}); });

            // ordenar los numeros aleatorios
            var new_order = general.sort(()=> Math.random()-0.5);
            this.group_value = new_order;

            var add_second=0;
            var add_minute=0;
            var tag_second=0;
            var tag_minute=0;

            // var count_players = this.players.length;
            // console.log(this.setup_players);
            if(this.setup_players==1){
                this.current_time = setInterval(()=>{
                    if(add_second==60){
                        add_minute++;
                        if(add_minute<10){
                            tag_minute="0"+add_minute;
                        }else{
                            tag_minute=add_minute;
                        }
                        add_second=1;
                        tag_second="00";
                    }else{
                        if(add_second<10){
                            tag_second="0"+add_second;
                        }else{
                            tag_second=add_second;
                        }
                        add_second++;
                    }
                    this.time_begin=tag_minute+":"+tag_second;
                },1000);
            }else{
                for(let i=1 ; i<=this.setup_players;i++){
                    this.point_player.push({name_player:i,point_game:0,css_select:"another_box_player"});
                }
                this.point_player[this.turn_up].css_select="another_box_class";
            }

        },
        capture(index){

            if(!this.save_index.includes(index)){
                var new_value = this.group_value[index].item;
                this.group_value[index].class_circle="style_cell_wrong";
                this.group_value[index].stop_circle=true;

                this.circle_index.push(index);
                var here = this.circle_index;
                var mirror =here.length;
                this.cap_number.push(new_value);
                var mark = this.cap_number.length;
                if(mark==2){
                    var the_same = this.cap_number[0]==this.cap_number[1];
                    if(the_same==true){
                        this.group_value[here[mirror-1]].class_circle="style_cell_selected";
                        this.group_value[here[mirror-2]].class_circle="style_cell_selected";
                        this.group_value[here[mirror-1]].stop_circle=true;
                        this.group_value[here[mirror-2]].stop_circle=true;
                        this.save_index.push(here[0],here[1]);
                        this.cap_number=[];
                        var finish = this.save_index.length;
                        var limit = this.group_value.length;

                        if(this.setup_players==1){
                            this.move_player++;
                        }else{
                            if(this.turn_up>=this.setup_players){
                                this.turn_up=0;
                            }
                            this.point_player[this.turn_up].point_game++;
                        }
                        
                        if(finish==limit){
                            clearInterval(this.current_time);
                            this.css_result_game={"visibility":"visible"};

                            this.point_player.map((x)=>{
                                this.finish_game.push(x.point_game);
                            });
                            this.value_winner = Math.max(...this.finish_game);

                        }
                    }else{
                        console.log(this.setup_players);
                        if(this.setup_players>1){
                            if(this.turn_up>=this.setup_players){
                                this.turn_up=0;
                            }else{
                                this.turn_up++;
                                if(this.turn_up==this.setup_players){
                                    this.turn_up=0;
                                }
                            }
                            this.point_player.map((x)=>{
                                x.css_select="another_box_player";
                                });
                                
                            this.point_player[this.turn_up].css_select="another_box_class";
                        }
                        

                        this.move_player++;
                        this.cap_number=[];
                        this.circle_index=[];

                        this.group_value[here[mirror-1]].class_circle="style_cell";
                        this.group_value[here[mirror-2]].class_circle="style_cell";
                        this.group_value[here[mirror-1]].stop_circle=false;
                        this.group_value[here[mirror-2]].stop_circle=false;
                    }
                    
                }
            }
        }
    }
};
</script>
<style scoped>
.box{
    width: 100%;
}
/* cuadro de resultados */
.result_game{
    position: fixed; 
    background-color: rgba(0, 0, 0, 0.4);
    width: 100%;
    height: 100vh;
    top:0px;
    left: 0px;
    display:flex;
    justify-content: center;
    align-items: center;
    z-index: 100;
}
.result_personal{
    background-color: white;
    border-radius: 8px;
    border: 0px solid black;
    width: 40%;
    padding: 30px;
}
.result_tag{
    font-weight: 900;
    color: #30495A;
    font-size: 18px;
    font-family: Arial, Helvetica, sans-serif;
    width: 50%;
    height: 100%;
    float: left;
    text-align: end;
}
.data_result{
    color: #30495A;
    font-weight: 600;
    font-size: 25px;
    font-family: Arial, Helvetica, sans-serif;
    text-align: center;
}
.data_result_second{
    color: #6E92A3;
    font-weight: 600;
    font-size: 18px;
    font-family: Arial, Helvetica, sans-serif;
    text-align: center;
}
/* fin de resultados */
.game_form{
    background-color: #172939;
    width: 100%;
    height: 100vh;
    display:flex;
    justify-content: center;
    align-items: center;
}
.game_data{
    background-color: white;
    border-radius: 8px;
    border: 0px solid black;
    width: 600px;
    padding: 30px;
}
.frame_game{
    margin-top: 70px;
}
.game_box{
    margin-top: 50px;
    margin-bottom: 50px;
    display: grid;
    justify-content: center;
    border: 0px solid black;
}
.style_cell{
    background-color: #172939;
    border-radius: 100px;
    color: #172939;
    font-family: Arial, Helvetica, sans-serif;
    font-size: 24px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 6px;
}
/* .style_cell:hover{
    background-color: #172939;
    color: white;
} */
.style_cell_selected{
    background-color: #6E92A3;
    border-radius: 100px;
    color: white;
    font-family: Arial, Helvetica, sans-serif;
    font-size: 24px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 6px;
}
.style_cell_wrong{
    background-color: #F48925;
    border-radius: 100px;
    color: white;
    font-family: Arial, Helvetica, sans-serif;
    font-size: 24px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 6px;
}
.data_theme{
    color: #30495A;
    font-weight: 600;
    font-size: 18px;
    font-family: Arial, Helvetica, sans-serif;
}
.join_button{
    display:flex;
}
.data_button{
    color: white;
    background-color: #6E92A3;
    border-radius: 30px;
    border: 0px solid black;
    padding: 10px;
    font-size: 20px;
    font-family: Arial, Helvetica, sans-serif;
    margin-right: 10px;
    transition: 0.5s;
    width: 50%;
}
.data_button_push{
    color: white;
    background-color: #172939;
    border-radius: 30px;
    border: 0px solid black;
    padding: 10px;
    font-size: 20px;
    font-family: Arial, Helvetica, sans-serif;
    margin-right: 10px;
    transition: 0.5s;
    width: 50%;
}
.send_game{
    color: white;
    background-color: #172939;
    border-radius: 30px;
    border: 0px solid black;
    padding: 10px;
    font-size: 25px;
    font-family: Arial, Helvetica, sans-serif;
    transition: 0.5s;
    width: 100%;
    margin-top: 25px;
}
.send_game:hover{
    color: white;
    background-color: #F48925;
}
.data_button:hover{
    color: white;
    background-color: #6E92A3;
}
.inbox{
    display: flex;
    flex-wrap: no-wrap;
}
.another_box{
    display: flex;
    justify-content: center;
    width: 100%;
}
.another_box_player{
    width: 500px;
    margin: 10px;
    border-radius: 10px;
    border: 0px solid black;
    background-color: #BCCFD9;
    color: #30495A;
    display: flex;
    padding: 10px;
}
.another_box_class{
    width: 500px;
    margin: 10px;
    border-radius: 10px;
    border: 0px solid black;
    background-color: #FDA215;
    color: white;
    display: flex;
    padding: 10px;
}
.game_tag{
    font-weight: 900;
    font-size: 14px;
    color: #6E92A3;
    font-family: Arial, Helvetica, sans-serif;
    width: 50%;
    height: 100%;
    display: flex;
    align-items: center;
}
.deposit{
    font-weight: 900;
    color: #30495A;
    font-size: 30px;
    font-family: Arial, Helvetica, sans-serif;
    width: 50%;
    height: 100%;
    float: left;
    text-align: end;
}
.inbox>div{
    width: 100%;
    text-align: center;
}
.side_left{
    font-weight: 900;
    font-size: 32px;
    font-family: Arial, Helvetica, sans-serif;
    color: #30495A;
}
.button_game{
    margin-left: 16px;
    padding: 15px;
    font-weight: 900;
    font-size: 16px;
    font-family: Arial, Helvetica, sans-serif;
    border-radius: 40px;
    border: 0px solid black;
    background-color: #BCCFD9;
    transition: 0.5s;
    color: #30495A;
    height: 100%;
}
.button_game:hover{
    background-color: #FDA215;
    color: white;
}
</style>