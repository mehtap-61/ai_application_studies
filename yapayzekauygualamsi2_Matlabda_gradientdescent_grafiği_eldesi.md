f=figure;hold on;

set(f,'defaultLineLineWidth',2);

syms w x y z;

 w=x*x+y*y-z;
 
dwdx=diff(w,x);

dwdy=diff(w,y);

dwdz=diff(w,z);

z=solve(w,z);

ezsurf(z,[-1,1]);

noktax=0.4;

noktay=0.3;

noktaz=subs(subs(z,x,noktax),y,noktay);

scatter3(noktax,noktay,noktaz);

gradx=subs(dwdx,x,noktax);

grady=subs(dwdy,y,noktay);

gradz=subs(dwdz,z,noktaz);

quiver3(noktax,noktay,noktaz,gradx,0,0);

quiver3(noktax,noktay,noktaz,0,grady,0);

quiver3(noktax,noktay,noktaz,0,0,gradz);

quiver3(noktax,noktay,noktaz,gradx,grady,gradz);

xlim([-1 1]);

ylim([-1 1]);

zlim([-1 1]);

view(151,8);

axis equal;

duyarlilik=0.01;

adimK=0.1;

maxAdimSayisi=100;

syms f x y

degiskenler=[x y];

f=x^2+y^2+1;

figure,colormap(gray),axis equal,hold on;

ezsurf(f,[-1.1 1.1]);

sekil=findobj('EdgeColor','black');
set(sekil,'LineStyle','none')

gradyan=[diff(f,x) diff(f,y)];

nokta=[1 1];

oncekiNokta=zeros(1,2);
z=1;

farklar=ones(1,2);

adimSayisi=0;

iniseDevam=true;
while iniseDevam && adimSayisi<maxAdimSayisi
  
    iniseDevam=false;
    
    for i=1:2 
        if farklar(i)>duyarlilik 
            adimSayisi=adimSayisi+1;

         
            iniseDevam=true;
            
            oncekiNokta=nokta; 
            nokta(i)=nokta(i)-adimK*subs(gradyan(i),degiskenler(i),nokta(i));

            farklar(i)=abs(oncekiNokta(i)-nokta(i));
        end
        oncekiZ=z;
            
        z=subs(subs(f,x,nokta(1)),y,nokta(2));

        scatter3(nokta(1),nokta(2),z,'ored','filled');
 
        if adimSayisi>1
            line([oncekiNokta(1) nokta(1)],[oncekiNokta(2) nokta(2)], ...
                 [oncekiZ z],'Color','cyan','Linewidth',2);
        end
    end
end
view(170,-4); 
fprintf('Atılan adım sayısı: %i\n',adimSayisi);
fprintf('Minimum nokta: (%f,%f)\n',nokta(1),nokta(2));
