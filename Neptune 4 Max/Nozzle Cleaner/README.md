To add a nozzle cleaner on the Neptune 4 Max.
I've used this model :
==> https://www.printables.com/model/1150288-neptune-4-max-nozzle-cleaner-wpositive-locator/files
This model is great because it insert in the bed and is locked by a screw of the bed.
Add a little coper brush that fit in the model.

*** In my case i've made a different X left case (less large). ***

*** Now we need to modify the printer.cfg to add the Macro : ***

[gcode_macro clean_nozzle]
gcode:
  {% set LOWERED_EXTRUDER_TEMP = params.EXTRUDER_TEMP|default(180)|int-40 %}
  {% set WIPES = params.WIPES|default(10)|int %}
  #Position the nozzle
  G90
  G0 X4.25 Y20 Z10 F5000
  #Heat up nozzle to extruder temperature - 40°C
  {action_respond_info("Clean nozzle: Heating up to {}°C the do {} wipes.".format(
    (LOWERED_EXTRUDER_TEMP),
    (WIPES)
  ))}
  M109 S{LOWERED_EXTRUDER_TEMP}
  #Bring the nozzle down
  G0 Z4
  #Swing the nozzle
  {% for wipe in range(WIPES) %}
      G0 Y5 F3000
      G0 Y35 F3000
  {% endfor %}
  #Bring the nozzle up
  G0 X4.25 Y20 Z10 F5000
  # Home to get the correct offset
  G28

*** And to use it before every print, i've added this in the printer setting of the Elegoo Slicer (based on Orca) : ***
Go in the machine g-code, in the start g-code, i've added it between the Nozzle start temp (M104 S140) and the bed start temp (M190 S[bed_temperature_initial_layer_single]) like that :

M104 S140
CLEAN_NOZZLE
M190 S[bed_temperature_initial_layer_single]

if need this is my complete start GCODE in orca :
;;===== date: 20240520 =====================
M400 ; wait for buffer to clear
;[printer_model]
;initial_filament:{filament_type[initial_extruder]}
;curr_bed_type={curr_bed_type}
FLASHLIGHT_SWITCH
M220 S100 ;Set the feed speed to 100%
M221 S100 ;Set the flow rate to 100%
M104 S140
CLEAN_NOZZLE
M190 S[bed_temperature_initial_layer_single]
G90
G28 ;home
G1 Z10 F300
G1 X{print_bed_max[0]*0.5-50} Y0.5 F6000
G1 Z0.4 F300
M109 S[nozzle_temperature_initial_layer]
G92 E0 ;Reset Extruder
G1 X{print_bed_max[0]*0.5+50} E30 F400 ;Draw the first line
G1 Z0.6 F120.0 ;Move to side a little
G1 X{print_bed_max[0]*0.5+47} F3000
G92 E0 ;Reset Extruder
;LAYER_COUNT:[total_layer_count]
;LAYER:0
