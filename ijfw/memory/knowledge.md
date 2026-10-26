<!-- ijfw-schema: v1 -->
# Knowledge Base
---
type: decision
summary: invaders3D camera limits + title sweep
stored: 2026-09-25T09:30:26.982Z
hash: 8e6a0271e622
tags: [camera, indexhtml, title-screen]
---
<!-- hash:8e6a0271e622 -->
invaders3D camera rework: camYaw clamped ±60° (CAM_MIN/MAX = ±PI/3), added camPitch/camPitchTarget clamped 0..+30° (PI/6), pitch keys R/F. Title screen sweep: titlePhase += 0.004, camYawTarget = sin(titlePhase) * CAM_MAX * 0.98 (continuous left↔right), eased lerp slowed to 0.06 yaw / 0.08 pitch; pitch raises camera Y (+pitch*55) and lookAt Y (+pitch*30).
---
type: decision
summary: invaders3D pitch tilts world group on player pivot
stored: 2026-09-25T09:47:13.237Z
hash: c1ac8734a1e6
tags: [camera, pitch, world-group]
---
<!-- hash:c1ac8734a1e6 -->
invaders3D pitch = world tilt: added `world` THREE.Group containing all playfield objects (grid, base, playerGrp, aliens, bunkers, bullets, UFO, particles); stars/lights stay in scene. updateTilt() sets world.rotation.x = camPitch with pivot compensation world.position.set(0, TILT_Z*sin, TILT_Z*(1-cos)) where TILT_Z = wz(H-22) (player row) so player stays fixed while invader rows (negative z) elevate when looking up (R key, max 30°).
---
type: decision
summary: invaders3D view presets, FOV, roll, zoom
stored: 2026-09-25T09:49:57.920Z
hash: ef313bfb8ecb
tags: [camera, fov, presets, hud]
---
<!-- hash:ef313bfb8ecb -->
invaders3D view options: VIEW_MODES array (DEFAULT 64fov/55dist/30h/150ahead; WIDE 84; ISO 35/95/90/60; TOP-DOWN 55/40/150h/10; CINEMATIC 45/70/16/130), keys 1-5 switch, +/= and -/_ adjust FOV 25-110, Z/C roll ±20° (X resets), wheel zoom camDistMul 0.5-2. All smoothed lerp 0.08. updateProjection() applies camera.fov each frame; HUD #hud-view shows mode+FOV.
---
type: decision
summary: invaders3D sprites now gap-free face-culled meshes
stored: 2026-09-25T09:56:28.797Z
hash: 33ee7a029ca5
tags: [voxel, geometry, sprites]
---
<!-- hash:33ee7a029ca5 -->
invaders3D solid sprites: spriteGeometry() rewritten from per-voxel cubes (VOX=S*0.92, gaps) to a face-culled merged mesh: occupancy grid from '#' chars, 6 face templates with neighbour offsets (±X/±Y culled when neighbour occupied; ±Z always exposed since sprites are 1 voxel deep), corners ±S/2, only exposed faces emitted. Sprite rows map to +Y (row y → cz via wz-style mapping, y up = -rows).
---
type: decision
summary: invaders3D horizontal FOV always fullscreen
stored: 2026-09-25T10:01:25.349Z
hash: 4c2268a30163
tags: [camera, fov, aspect]
---
<!-- hash:4c2268a30163 -->
invaders3D FOV is now HORIZONTAL (HFOV 40-150°, keys +/-). updateProjection() converts hFov→vertical per aspect: vFov = 2*atan(tan(hFov/2)/aspect) so render always fills screen at any aspect ratio. Presets re-tuned: DEFAULT 96, WIDE 112, ISO 58, TOP-DOWN 85, CINEMATIC 68 (all horizontal). HUD shows HFOV.
---
type: decision
summary: invaders3D FOV never moves camera; V/B roll
stored: 2026-09-25T10:03:35.776Z
hash: c88709be802d
tags: [camera, fov, roll]
---
<!-- hash:c88709be802d -->
invaders3D FOV-vs-camera separation: camera position/lookAt derive ONLY from view preset (m.dist/height/ahead) * camDistMul + camPitch tilt — camFov never feeds position, so FOV changes zoom optically without moving camera. Extra roll keys V/B added alongside Z/C, X still resets. Alt FOV keys T (wider) / G (narrower) alongside +/-.
---
type: decision
summary: invaders3D HUD live camera params readout
stored: 2026-09-25T10:40:35.651Z
hash: 7c0ee8cf005f
tags: [hud, camera]
---
<!-- hash:7c0ee8cf005f -->
invaders3D HUD camera readout: #hud-view shows VIEW preset name, HFOV, YAW (deg), PITCH (deg), ZOOM multiplier, DIST (dist*zoom), H (effective camera height incl pitch raise). Cache signature extended with yaw/pitch/zoom so it refreshes live.
