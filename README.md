import React, { useCallback, useMemo, useState } from 'react';
import ReactFlow, { Background, Controls, MiniMap } from 'reactflow';
import 'reactflow/dist/style.css';
import cursos from './cursos.json';

const nivelY = 150; // Distancia vertical entre niveles
const nodoX = 200;  // Distancia horizontal entre nodos

function App() {
  const nodes = useMemo(() => {
    let nivelContador = {};
    return cursos.map((curso) => {
      const x = (nivelContador[curso.nivel] || 0) * nodoX;
      const y = (curso.nivel - 1) * nivelY;
      nivelContador[curso.nivel] = (nivelContador[curso.nivel] || 0) + 1;
      return {
        id: curso.codigo,
        position: { x, y },
        data: { label: `${curso.codigo}\n${curso.nombre}` },
        type: 'default',
      };
    });
  }, []);

  const edges = useMemo(() => {
    return cursos.flatMap((curso) =>
      curso.prerrequisitos.map((pre) => ({
        id: `${pre}-${curso.codigo}`,
        source: pre,
        target: curso.codigo,
        type: 'smoothstep',
      }))
    );
  }, []);

  return (
    <div style={{ width: '100vw', height: '100vh' }}>
      <ReactFlow nodes={nodes} edges={edges} fitView>
        <MiniMap />
        <Controls />
        <Background />
      </ReactFlow>
    </div>
  );
}

export default App;
