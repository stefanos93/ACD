# ACD
Analog Circuit Design
ACD – FORMULARIO

ANALOG STAGE DESIGN
MOSFET PARAMETERS
Ohmic Current:		I_(DS,ohm)=μC^'ox(W/L) V_DS (V_OV-V_DS/2)
Saturation Current:	I_(DS,sat)=1/2  μC^' ox(W/L) (V_GS-V_T )^2 (1+V_DS/V_A )
Small Signal Parameters:	gm=(2I_bias)/V_OV 		r_0=V_A/I_bias 

Source Resistance:	R_S=(r_0+R_d)/(1+gmr_0 )
Drain Resistance:	R_D=r_0+R_s+gmr_0 R_s
Moderate/Weak Inversion:	IC=I_D/I_S =I_D/(2nμC^' oxV_TH^2 (W/L) )           gm=I_D/(nV_TH )  2/(1+√(1+4IC))          n=3/2
Taxonomy	IC values	Bias range
Weak inversion	IC≤0.1	V_GS≤V_T-0.1V
Moderate inversion	0.1≤IC≤10	V_T-0.1≤V_GS≤V_T-0.2V
Strong inversion	IC≥10	V_GS≥V_T+0.2V
Cut-off Frequency:	f_p=gm/2π(C_GS+C_GD ) 
THERMAL NOISE SOURCES
	Resistor:	S_v=4k_b TR
	Mosfet:		S_I=4k_B Tγgm          where γ=1 in ohmic,γ=2/3  in saturation (2 if short ch)
MOSFET BASIC CONFIGURATIONS
	Common Source: 	G=-gm(r_0∥R_d )
	Source Degenerated:	G=-(gmR_d)/(1+gmR_s+(R_d+R_s)/r_0 )≅-(gmR_d)/(1+gmR_s )           if R_d+R_s≪r_0
	Source Follower:	G=
PROTOTYPICAL DIFFERENTIAL STAGE
With Resistive Load
Differential Mode:	I_cc=(gm_d v_d)/2          R_out=R_L           G_d=(gm_d)/2  R_L	
Common Mode:		I_cc=v_cm/(2R_E )           〖R_out=R_L           G〗_cm=R_L/(2R_E )           
CMRR=G_d/G_cm =gm_d R_E
With Active (Mirror) Load
Differential Mode:	I_cc=gm_d v_d          R_out=r_0m∥(2r_0d)/(1-(-1) )           G_d=gm_d  r_0m∥r_0d
Common Mode:		I_cc=ϵi_cm           R_out=r_0m∥r_0d           G_cm=(ϵR_out)/(2r_0g )
CMRR=G_d/G_cm =(2g_md r_og)/ϵ
White Noise:		E_n^2=2/(g_md^2 ) (4k_B Tγg_md+4k_B Tγg_mm )
TWO STAGE OTA
	Differential Gain:	G_d=gm_d  (r_0m∥r_0d )  g_m2s (r_02s∥r_0g2s)
	Compensations
	Miller Capacitance:	f_1=1/2π[R_out1 (C_1+C_M (1+gm_2s R_out2 )+(C_M+C_L ) R_out2 ] ≈1/2π[R_out1 C_M gm_g2s R_out2 ] 
				f_2=(C_M R_out1 gm_2s R_out2)/(2π[R_out1 C_1 R_out2 (C_L+C_M )+R_out2 C_L C_M R_out1])=(C_M gm_2s)/(C_M (C_1+C_L )+C_1 C_L )→(gm_2s)/(C_1+C_L )
				GBWP=(gm_d)/(2πC_M )           f_z=(gm_2s)/(2πC_M )            f_3=∞
			 
# to have stability both the second pole and the positive zero should be moved away from the GBWP, so increasing C_M and reducing the GBWP and the zero that follows it (at least a decade below f_2 ), or increasing the transconductance of the second stage, to move the zero away from the GBWP together with the second pole.

Nulling Resistor:	f_z=1/(2πC_M (1/(gm_2s )-R_N ) )           f_3=(1/C_1 +1/C_L +1/C_M )/(2πR_N )
# if we set R_N=1/(gm_2s ) the zero is pushed to infinite. Better if we set R_N>1/(gm_2s ) so that the zero becomes even negative, best solution put f_z=f_2 to have a cancellation in phase (-90+90). Starting setting C_M=(gm_d)/(gm_2s ) (C_1+C_L ) to have GBWP=f_2, then R_N=1/(gm_d )+1/(gm_2s ) to have the pole-zero compensation.
To build R_N for pole-zero compensation  with a transistor M_R with I_bias=0 and gm_R=2k(w/L)_R V_OVR we need to derive (W/L)_R from gm_R=(1+(gm_2s)/(gm_d ))gm_2s, and if V_OVR=V_OV2s we have (W/L)_R=((W/L)_5 gm_R)/(gm_2s ).

Ahuja Compensation:	f_2=(gm_2s)/C_1            f_2=(gm_2s)/C_L  (if we use a voltage buffer)          f_z=∞
			f_2/GBWP=(gm_2s)/(gm_d )  C_M/C_1   C/(C_M+C_2 )→(gm_2s)/(gm_d )  C_M/C_1 
# for C_M values larger than the load capacitance, the second pole is set by C_1, which is usually much smaller than C_M and C_L. Therefore, the (gm_2s)/(gm_d ) ratio needed to reach a given phase margin can be much lower than the value required in the nulling resistor case, thus leading to lower current consumption.
# Finite buffer resistance introduces a left zero and a new HF pole that can become conjugated with the second pole: f_z=-1/(2πC_1 R_out1 )  or f_z=-(gm_B)/(2πC_M ) (if we use a voltage buffer). In the case of Ahuja we can estimate pole positions from R_out expression R_out2=1/(gm_2s gm_B R_out1 )  (1+sC_1 R_out1)/((s^2  (C_1 C_L)/(gm_B gm_2s )+s C_1/(gm_2s )+1) ).
Deriving ω_0=√((gm_2s gm_B)/(C_1 C_L )) and Q=√((gm_2s C_L)/(gm_B C_1 )). This couple of poles degrade the PM of about 
 Δϕ=-arctg((GBWPf_0)/Q(f_0^2-GBWP^2 ) ), being f_0=ω_0/2π.
Cascode Compensation:	it has the same poles of Ahuja compensation (being gm_B=gm_cascode), and it has two opposite zeroes at the same frequency: f_z=±√((gm_2s gm_B)/(C_M C_1 ))  1/2π if not used in Cascoded Mirror compensation. 
Slew-Rate:	SR_int=I_max/C_M            where I_max  is often due to the tail generator of the first stage
		SR_int=I_max/(gm_d )  (2πGBWP)
# when we have a two stage amplifier, on the positive slope of the output the limit is set by SR_int, on the negative one is the minimum between SR_int and I_2s/(C_M+C_L ).
SINGLE STAGE AMPLIFIERS
Telescopic cascode amplifier:	〖I_cc=gm_d v_d           G〗_d=(gm_d gm_cas r_ocas^2)/2
Folded cascode amplifier:	I_cc=gm_d v_d           G_d=(gm_d gm_cas r_0cas)/3
CMRR AND OFFSET
