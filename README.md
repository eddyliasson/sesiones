<?php 
session_start();
if ($_SERVER["REQUEST_METHOD"]=="POST") {
    if (isset($_POST["enviar"])) {
        $cedula=$_POST["cedula"];
        $nombre=$_POST["nombre"];
        $apellido=$_POST["apellido"];
        $autonomo=$_POST["autonomo"];
        $colab=$_POST["colab"];
        $guias=$_POST["guias"];
        $evaluacion=$_POST["eval"];

$nuevoAlumno=[
'cedula'=>$cedula,
'nombre'=>$nombre,
'apellido'=>$apellido,
'autonomo'=>$autonomo,
'colaborativo'=>$colab,
'guias'=>$guias,
'evaluacion'=>$evaluacion,
        ];
        $_SESSION['alumnos'][]=$nuevoAlumno;
    }elseif (isset($_POST["eliminar"])) {
        $_SESSION['alumnos']=[];
    }elseif (isset($_POST["eliminar_cedula"])){


$cedula_eliminar=$_POST["ceduladelete"];

foreach ($_SESSION ['alumnos']as $delete=>$alumno) {
if($alumno['cedula']==$cedula_eliminar){

unset($_SESSION['alumnos'][$delete]);
$_SESSION['alumnos']=array_values($_SESSION['alumnos']);

}

}
    }
}

$ali=isset($_SESSION['alumnos'])? $_SESSION['alumnos']:[];

foreach($ali as&$alu){
$alu['suma']= $alu['autonomo']*0.2+$alu['colaborativo']*0.3+$alu['guias']*0.2+$alu['evaluacion']*0.3;

}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="style.css">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro Notas</title>
</head>
<body>
    <h2>INGRESO DE DATOS - TUVN</h2>
    <form action="<?php echo htmlspecialchars ($_SERVER["PHP_SELF"]);?>" method="POST"> 
   
<label for="">CEDULA:</label>
<input type="number" name="cedula" require>
<br>

<label for="">NOMBRE:</label>
<input type="text" name="nombre" require>
<br>

<label for="">APELLIDO:</label>
<input type="text" name="apellido" require>
<br>

<label for="">PROMEDIO AUTONOMO:</label>
<input type="number" name="autonomo" require>
<br>

<label for="">PROMEDIO COLABORATIVO:</label>
<input type="number" name="colab" require>
<br>

<label for="">PROMEDIO GUIAS:</label>
<input type="number" name="guias" require>
<br>

<label for="">EVALUACION:</label>
<input type="number" name="eval" require>
<br>

<button type="submit" name="enviar">ENVIAR</button>
<button type="submit" name="eliminar">Eliminar Lista</button>
<br>
<br>
<br>
<label for="">Eliminar por Cedula</label>
<input type="number" name="ceduladelete">
<button type="submit" name="eliminar_cedula">Eliminar por cedula</button>
</form>

<table>
    <thead>
        <tr>
            <th>CEDULA</th>
            <th>NOMBRE</th>
            <th>APELLIDO</th>
            <th>AUTONOMO</th>
            <th>COLABORATIVO</th>
            <th>GUIAS</th>
            <th>EVALUACIONES</th>
            <th>PROMEDIO</th>
            <th>ESTADO</th>
        </tr>
    </thead>
<tbody>
<?php foreach ($ali as $alu): ?>
<tr>

<td><?php echo $alu['cedula'] ?> </td>
<td><?php echo $alu['nombre'] ?> </td>
<td><?php echo $alu['apellido'] ?> </td>
<td><?php echo $alu['autonomo'] ?> </td>
<td><?php echo $alu['colaborativo'] ?> </td>
<td><?php echo $alu['guias'] ?> </td>
<td><?php echo $alu['evaluacion'] ?> </td>
<td><?php echo $alu['suma'] ?> </td>
<td><?php  if ($alu['suma']>7) {
    echo "Aprobado";

}else{

echo "Reprobado";

}?> </td>

</tr>
<?php endforeach; ?>
</tbody>

</tbody>

</table>
</body>
</html>
