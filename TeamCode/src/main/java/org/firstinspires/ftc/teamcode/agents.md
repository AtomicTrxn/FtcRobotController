# FTC Robot Controller Architecture & TeamCode Guide

This document provides essential context for AI agents and developers working within the `TeamCode` module of the FTC Robot Controller project.

## Project Structure Overview

- **`:FtcRobotController` Module:** Contains the SDK infrastructure, the main `Activity` and `Service` for the Robot Controller app, and a vast collection of sample OpModes. **Avoid modifying this module.**
- **`:TeamCode` Module:** The designated area for all custom team code. Located at `src/main/java/org/firstinspires/ftc/teamcode`.

## Core Concepts

### OpModes
An OpMode (Operational Mode) is a program for the robot. There are two primary styles:

1. **LinearOpMode:** A procedural approach. Best for Autonomous or simple TeleOp.
   - **Main Method:** `runOpMode()`
   - **Key Commands:** `waitForStart()`, `opModeIsActive()`, `idle()`, `sleep()`.
   - **Flow:** `Init` -> `Wait for Start` -> `Loop while Active` -> `Stop`.

2. **OpMode (Iterative):** An event-driven approach. Good for complex state machines.
   - **Key Methods:** `init()`, `init_loop()`, `start()`, `loop()`, `stop()`.
   - **Flow:** `init()` is called once. `loop()` is called repeatedly after the driver presses Start.

### Hardware Mapping
Hardware is defined in a configuration file on the Robot Controller device and accessed via the `hardwareMap`.
- **Usage:** `DcMotor motor = hardwareMap.get(DcMotor.class, "motor_name");`
- **Common Types:** `DcMotor`, `Servo`, `ColorSensor`, `IMU`, `DistanceSensor`, `TouchSensor`.

### Gamepads
Two gamepads are available: `gamepad1` and `gamepad2`.
- **Inputs:** `left_stick_x/y`, `right_stick_x/y`, `left_trigger`, `right_trigger`, `a`, `b`, `x`, `y`, `dpad_up/down/left/right`, `left_bumper`, `right_bumper`.

### Telemetry
Used to send status data to the Driver Station.
- **Usage:** `telemetry.addData("Key", "Value"); telemetry.update();`

## Development Workflow

1. **Copy Samples:** The easiest way to start is to copy a sample from `FtcRobotController/java/external/samples` into the `org.firstinspires.ftc.teamcode` package.
2. **Annotation:** Use `@TeleOp` or `@Autonomous` to register your class.
   - Example: `@TeleOp(name="My Robot Drive", group="Competition")`
3. **Registration:** By default, new classes are disabled. **Comment out or delete the `@Disabled` annotation** to make them appear on the Driver Station.
4. **Configuration:** Ensure the strings used in `hardwareMap.get()` match the names in your robot's configuration on the controller phone/Control Hub.

## Key SDK Classes (Reference)

- `com.qualcomm.robotcore.eventloop.opmode.LinearOpMode`
- `com.qualcomm.robotcore.eventloop.opmode.OpMode`
- `com.qualcomm.robotcore.hardware.DcMotor`
- `com.qualcomm.robotcore.hardware.Servo`
- `com.qualcomm.robotcore.hardware.HardwareMap`
- `com.qualcomm.robotcore.util.ElapsedTime`
- `com.qualcomm.robotcore.util.Range` (useful for clipping motor powers)

## Architecture Summary

The `FtcRobotControllerActivity` serves as the UI and entry point, while the `FtcRobotControllerService` manages the lifecycle of the robot and hardware communication. When an OpMode is selected and started, the SDK instantiates your class and manages its execution according to the OpMode type (Linear vs. Iterative).
