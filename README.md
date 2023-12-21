# flappybird-rap001-.github.io
# PROJECT PYTHON :FLAPPY BIRD

import pygame
from random import randint
pygame.init()
screen=pygame.display.set_mode((800,700))
pygame.display.set_caption('nhay di chim')
clock=pygame.time.Clock()
fps=60
background_img=pygame.image.load('img/bg.png')
background_img=pygame.transform.scale(background_img,(800,700))
#scrore
score=0
font=pygame.font.SysFont('san',50)
#game over
font1=pygame.font.SysFont('san',50)
#tao
tao=pygame.image.load('img/15.png')

#score_tao
score_tao=0
font4=pygame.font.SysFont('san',50)
#bird
x_bird=112
y_bird=350
bird_drop_velocity=0
bird_gravity=0.3
#tube
tube1_x=400
tube2_x=650
tube3_x=850
tao_height=tube1_height=randint(200,300)
tube2_height=randint(250,300)
tube3_height=randint(150,250)
#tao
tao=pygame.image.load('img/15.png')
tao_x= 400 +100
tao_height=tube1_height+100
#tube opposite
tube1_op_height=randint(200,300)
tube2_op_height=randint(250,300)
tube3_op_height=randint(150,250)
#ground
ground_image=pygame.image.load('img/ground.png')
tube_img=pygame.image.load('img/pipe1.jpg')
tube_op_img=pygame.image.load('img/pipe.png')
d_2tube=150
#bird img
bird_img=pygame.image.load('img/bird1.png')
bird_img=pygame.transform.scale(bird_img,(35,35))
WHITE=(255,255,255)
RED=(255,0,0)

velocity=2       
ground_scroll=0  
#sound
sound=pygame.mixer.Sound('img/no6.wav')
#kiem tra chim qua ong chua
tube1_pass=False 
tube2_pass=False   
tube3_pass=False   
#kiem tra bird trung tao chua
tao1_pass=False
#kiem tra ct dung chua
pausing=False             
running=True
while running:
    
    
    #mixer

    pygame.mixer.Sound.play(sound)
    clock.tick(60)
    screen.fill(WHITE)
    screen.blit(background_img,(0,0))
    
    

    #ep anh
    tube1_img=pygame.transform.scale(tube_img,(50,tube1_height))
    tube1=screen.blit(tube1_img,(tube1_x,0))
    tube2_img=pygame.transform.scale(tube_img,(50,tube2_height))
    tube2=screen.blit(tube2_img,(tube2_x,0))
    tube3_img=pygame.transform.scale(tube_img,(50,tube3_height))
    tube3=screen.blit(tube3_img,(tube3_x,0))
    # ve ong doi dien
    tube1_op_img=pygame.transform.scale(tube_op_img,(50,600-d_2tube-tube1_height))
    tube1_op=screen.blit(tube1_op_img,(tube1_x,tube1_height+d_2tube))

    tube2_op_img=pygame.transform.scale(tube_op_img,(50,600-d_2tube-tube2_height))
    tube2_op=screen.blit(tube2_op_img,(tube2_x,tube2_height+d_2tube))
    
    tube3_op_img=pygame.transform.scale(tube_op_img,(50,600-d_2tube-tube3_height))
    tube3_op=screen.blit(tube3_op_img,(tube3_x,tube3_height+d_2tube))
    #tao
    tao_img=pygame.transform.scale(tao,(50,50))
    tao1=screen.blit(tao_img,(tube1_x+100,(tube1_height+100)))
    if tao_x<-50 :
        tao_x=tube1_x+100             
        tao_height= randint(100,400)  
        tao1_pass=False
    

    

    #trong luc cua chim
    y_bird=y_bird+bird_drop_velocity
    bird_drop_velocity=bird_drop_velocity+bird_gravity
    #bird
    bird=screen.blit(bird_img,(x_bird,y_bird))
    #ground
    ground=screen.blit(ground_image,(0,550))
    #ghi diem tao
    tao_txt=font.render('APPLE:'+str(score_tao),True,RED)
    screen.blit(tao_txt,(0,50))
    #ghi diem
    score_txt=font.render('Score:'+str(score),True,RED)
    screen.blit(score_txt,(5,5))
    # pipe lui ve
    tube1_x-=velocity
    tube2_x-=velocity
    tube3_x-=velocity
    if tube1_x<-50:
        tube1_x=700
        tube1_height=randint(100,400)
        tube1_pass=False
    if tube2_x<-50:
        tube2_x=700
        tube2_height=randint(100,400)
        tube2_pass=False
    if tube3_x<-50:
        tube3_x=700
        tube3_height=randint(100,400)
        tube3_pass=False
    #kiem tra chim qua pipe chua
    if tube1_x+50<=x_bird and tube1_pass==False:   
        score+=1
        tube1_pass=True

    if tube2_x+50<=x_bird and tube2_pass==False:   
        score+=1
        tube2_pass=True
    if tube3_x+50<=x_bird and tube3_pass==False:   
        score+=1
        tube3_pass=True
    # kiem tra bird trung tao chua
    
    if bird.colliderect(tao1) and tao1_pass==False:
        score_tao+=1 
        tao1_pass=True 
          
    # kiem tra va cham
    tubes=[tube1,tube2,tube3,tube1_op,tube2_op,tube3_op]
    for tube in tubes:
        if bird.colliderect(tube):
            

            #mixer
            pygame.mixer.pause()
            #velocity
            velocity=0
            bird_drop_velocity=0  
            game_over_txt=font1.render('Game Over, Score:'+str(score),True,RED)
            screen.blit(game_over_txt,(270,260))
            space_txt=font.render("Press Space to continue!",True,RED)
            screen.blit(space_txt,(230,300))
            #kiem tra pausing
            pausing=True
    if y_bird>=600 or y_bird<=0:
        #mixer
            pygame.mixer.pause()
            velocity=0
            bird_drop_velocity=0  
            game_over_txt=font1.render('Game Over, Score:'+str(score),True,RED)
            screen.blit(game_over_txt,(270,260))
            space_txt=font.render("Press Space to continue!",True,RED)
            screen.blit(space_txt,(230,300))
            #kiem tra pausing
            pausing=True

    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            running=False

        if event.type==pygame.KEYDOWN:
            if event.key==pygame.K_SPACE:
                bird_drop_velocity=0
                bird_drop_velocity-=7
                if pausing:
                    #reset lai
                    x_bird=50
                    y_bird=350
                    tube1_x=400
                    tube2_x=600
                    tube3_x=850
                    velocity=2
                    score=0
                    if score_tao<0:
                        score_tao=0
                    if score_tao>0:
                        score_tao-=1
                
                    pausing=False
                    #mixer
                    pygame.mixer.unpause()
               
    pygame.display.flip()

pygame.quit()    
