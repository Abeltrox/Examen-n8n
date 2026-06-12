# Examen-n8n Oscar Abel Bonilla Garcia

Sistema de Alerta de Stock Crítico
´´´´{
  "nodes": [
    {
      "parameters": {
        "updates": [
          "message",
          "callback_query"
        ],
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegramTrigger",
      "typeVersion": 1.3,
      "position": [
        -1744,
        320
      ],
      "id": "b310be48-76d5-4991-9ffd-6052da021325",
      "name": "Telegram Trigger",
      "webhookId": "334a1e71-d571-4f6b-9d23-66bc60a0a232",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const input = $input.first().json;\nlet chat_id, telegram_id, nombre, mensaje = '';\nif (input.callback_query) {\n  chat_id = input.callback_query.message.chat.id;\n  telegram_id = input.callback_query.from.id;\n  nombre = input.callback_query.from.first_name || '';\n  mensaje = input.callback_query.data || '';\n} else if (input.message) {\n  chat_id = input.message.chat.id;\n  telegram_id = input.message.from.id;\n  nombre = input.message.from.first_name || '';\n  mensaje = (input.message.text || '').trim();\n}\nreturn [{ json: { chat_id, telegram_id, nombre, mensaje } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1536,
        320
      ],
      "id": "614ec0f7-1878-4705-a77b-c019389b4449",
      "name": "Parser de Mensajes"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=2135519642#gid=2135519642",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 2135519642,
          "mode": "list",
          "cachedResultName": "USUARIOS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit#gid=2135519642"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "telegram_id": "={{ $json.telegram_id }}",
            "nombre_completo": "={{ $json.nombre }}",
            "puntos_lealtad": "0"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "telegram_id",
              "displayName": "telegram_id",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "nombre_completo",
              "displayName": "nombre_completo",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "departamento",
              "displayName": "departamento",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "puntos_lealtad",
              "displayName": "puntos_lealtad",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -1152,
        176
      ],
      "id": "3038ad8a-c8fe-4e76-9e48-f569e8bca245",
      "name": "Registrar Usuario",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.mensaje }}",
                    "rightValue": "ver_menu",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "0a518282-72c4-4903-bc20-4196621bf972"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "ver_menu"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.mensaje }}",
                    "rightValue": "cat_",
                    "operator": {
                      "type": "string",
                      "operation": "startsWith"
                    },
                    "id": "f002489f-4399-4257-8cf3-3242a3b9b68e"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "categorias"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.mensaje }}",
                    "rightValue": "add_",
                    "operator": {
                      "type": "string",
                      "operation": "startsWith"
                    },
                    "id": "add-rule-001"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "agregar_producto"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.mensaje }}",
                    "rightValue": "ver_carrito",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "carrito-rule-001"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "ver_carrito"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.mensaje }}",
                    "rightValue": "confirmar_pedido",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "confirmar-rule-001"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "confirmar_pedido"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "44654344-f450-43f0-b629-9f53dbaf8e89",
                    "leftValue": "={{ $json.mensaje }}",
                    "rightValue": "cancelar_pedido",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    }
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "Cancelar_pedido"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.mensaje }}",
                    "rightValue": "mis_pedidos",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "mispedidos-rule-001"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "mis_pedidos"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.mensaje }}",
                    "rightValue": "ayuda",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "ayuda-rule-001"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "ayuda"
            }
          ]
        },
        "options": {
          "fallbackOutput": "extra"
        }
      },
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.4,
      "position": [
        -1136,
        400
      ],
      "id": "367e3ded-6858-43c6-877b-e5652e882f65",
      "name": "Router Principal",
      "alwaysOutputData": true
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $('Parser de Mensajes').item.json.mensaje }}",
                    "rightValue": "cat_bebidas",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "e981eb75-b953-4dd2-b4b8-021cde5e97ad"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "bebidas"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $('Parser de Mensajes').item.json.mensaje }}",
                    "rightValue": "cat_almuerzos",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "4ac47708-7b9d-4aa1-96d0-7af0e7cc750d"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "almuerzos"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $('Parser de Mensajes').item.json.mensaje }}",
                    "rightValue": "cat_snacks",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "3c3d7f6a-ae9e-4604-b78c-944fb4f493d2"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "snacks"
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $('Parser de Mensajes').item.json.mensaje }}",
                    "rightValue": "cat_postres",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "58017c95-1b9f-45c1-b069-6ee6ad53d7da"
                  }
                ],
                "combinator": "and"
              },
              "renameOutput": true,
              "outputKey": "postres"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.4,
      "position": [
        -400,
        336
      ],
      "id": "8a4808c1-a104-428d-8385-bd83873a4a39",
      "name": "Switch Categorias"
    },
    {
      "parameters": {
        "jsCode": "const todos = $input.all();\nconst lista = [0,1,2].map(i => todos[i]?.json).filter(Boolean);\nreturn [{ json: { lista } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -208,
        160
      ],
      "id": "2cc363c3-4bd8-4af4-8ae6-a106b641608c",
      "name": "Filtrar Bebidas"
    },
    {
      "parameters": {
        "jsCode": "const todos = $input.all();\nconst lista = [3,4,5].map(i => todos[i]?.json).filter(Boolean);\nreturn [{ json: { lista } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -48,
        224
      ],
      "id": "65aeea1e-1e66-4040-aca5-9d23ff5c0d89",
      "name": "Filtrar Almuerzos"
    },
    {
      "parameters": {
        "jsCode": "const todos = $input.all();\nconst lista = [6,7,8].map(i => todos[i]?.json).filter(Boolean);\nreturn [{ json: { lista } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -80,
        368
      ],
      "id": "0aa7263f-55f4-437c-9bbe-98a157ec4647",
      "name": "Filtrar Snacks"
    },
    {
      "parameters": {
        "jsCode": "const todos = $input.all();\nconst lista = [9,10].map(i => todos[i]?.json).filter(Boolean);\nreturn [{ json: { lista } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        192,
        496
      ],
      "id": "8fce1899-947c-4fdb-a43a-854a1d409966",
      "name": "Filtrar Postres"
    },
    {
      "parameters": {
        "chatId": "={{ $('Parser de Mensajes').item.json.chat_id }}",
        "text": "🥤 *Elige la bebida que deseas:*",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "={{ $json.lista[0].nombre + ' - $' + $json.lista[0].precio }}",
                    "additionalFields": {
                      "callback_data": "={{ 'add_' + $json.lista[0].id_producto }}"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "={{ $json.lista[1].nombre + ' - $' + $json.lista[1].precio }}",
                    "additionalFields": {
                      "callback_data": "={{ 'add_' + $json.lista[1].id_producto }}"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "={{ $json.lista[2].nombre + ' - $' + $json.lista[2].precio }}",
                    "additionalFields": {
                      "callback_data": "={{ 'add_' + $json.lista[2].id_producto }}"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "🧺 Ver carrito",
                    "additionalFields": {
                      "callback_data": "ver_carrito"
                    }
                  },
                  {
                    "text": "🔙 Menú",
                    "additionalFields": {
                      "callback_data": "ver_menu"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        0,
        0
      ],
      "id": "4321ca36-985c-40a5-87dc-fa948daaff3c",
      "name": "Lista Bebidas",
      "webhookId": "88eea98a-fac3-48c0-9b41-2df13692e200",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Parser de Mensajes').item.json.chat_id }}",
        "text": "☀️ *Elige el almuerzo que deseas:*",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "={{ $json.lista[0].nombre + ' - $' + $json.lista[0].precio }}",
                    "additionalFields": {
                      "callback_data": "={{ 'add_' + $json.lista[0].id_producto }}"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "={{ $json.lista[1].nombre + ' - $' + $json.lista[1].precio }}",
                    "additionalFields": {
                      "callback_data": "={{ 'add_' + $json.lista[1].id_producto }}"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "={{ $json.lista[2].nombre + ' - $' + $json.lista[2].precio }}",
                    "additionalFields": {
                      "callback_data": "={{ 'add_' + $json.lista[2].id_producto }}"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "🧺 Ver carrito",
                    "additionalFields": {
                      "callback_data": "ver_carrito"
                    }
                  },
                  {
                    "text": "🔙 Menú",
                    "additionalFields": {
                      "callback_data": "ver_menu"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        176,
        144
      ],
      "id": "908c660a-8f3f-4360-8961-50ce759827a8",
      "name": "Lista Almuerzos",
      "webhookId": "88eea98a-fac3-48c0-9b41-2df13692e200",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Parser de Mensajes').item.json.chat_id }}",
        "text": "🍿 *Elige el snack que deseas:*",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "={{ $json.lista[0].nombre + ' - $' + $json.lista[0].precio }}",
                    "additionalFields": {
                      "callback_data": "={{ 'add_' + $json.lista[0].id_producto }}"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "={{ $json.lista[1].nombre + ' - $' + $json.lista[1].precio }}",
                    "additionalFields": {
                      "callback_data": "={{ 'add_' + $json.lista[1].id_producto }}"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "={{ $json.lista[2].nombre + ' - $' + $json.lista[2].precio }}",
                    "additionalFields": {
                      "callback_data": "={{ 'add_' + $json.lista[2].id_producto }}"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "🧺 Ver carrito",
                    "additionalFields": {
                      "callback_data": "ver_carrito"
                    }
                  },
                  {
                    "text": "🔙 Menú",
                    "additionalFields": {
                      "callback_data": "ver_menu"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        128,
        320
      ],
      "id": "8a419e5b-e4ab-4e88-85ac-5850254bc73d",
      "name": "Lista Snacks",
      "webhookId": "88eea98a-fac3-48c0-9b41-2df13692e200",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Parser de Mensajes').item.json.chat_id }}",
        "text": "🍰 *Elige el postre que deseas:*",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "={{ $json.lista[0].nombre + ' - $' + $json.lista[0].precio }}",
                    "additionalFields": {
                      "callback_data": "={{ 'add_' + $json.lista[0].id_producto }}"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "={{ $json.lista[1].nombre + ' - $' + $json.lista[1].precio }}",
                    "additionalFields": {
                      "callback_data": "={{ 'add_' + $json.lista[1].id_producto }}"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "🧺 Ver carrito",
                    "additionalFields": {
                      "callback_data": "ver_carrito"
                    }
                  },
                  {
                    "text": "🔙 Menú",
                    "additionalFields": {
                      "callback_data": "ver_menu"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        400,
        496
      ],
      "id": "59b47da3-f2ef-480e-9691-7d8d836ee63f",
      "name": "Lista Postres",
      "webhookId": "88eea98a-fac3-48c0-9b41-2df13692e200",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const parserData = $('Parser de Mensajes').item.json;\nconst mensaje = parserData.mensaje;\nconst id_producto = mensaje.replace('add_', '');\n\nconst sesiones = $input.all();\nlet carritoActual = [];\nlet rowNumber = null;\n\nif (sesiones.length && sesiones[0].json.telegram_id) {\n  const sesionUsuario = sesiones.find(s => String(s.json.telegram_id) === String(parserData.telegram_id));\n  if (sesionUsuario) {\n    rowNumber = sesionUsuario.json.row_number || null;\n    try {\n      carritoActual = JSON.parse(sesionUsuario.json.carrito_temporal || '[]');\n      if (!Array.isArray(carritoActual)) carritoActual = [];\n    } catch(e) { carritoActual = []; }\n  }\n}\n\nconst idx = carritoActual.findIndex(i => i.id === id_producto);\nif (idx >= 0) {\n  carritoActual[idx].cantidad += 1;\n} else {\n  carritoActual.push({ id: id_producto, cantidad: 1 });\n}\n\nreturn [{ json: {\n  telegram_id: parserData.telegram_id,\n  nombre: parserData.nombre,\n  chat_id: parserData.chat_id,\n  carrito_temporal: JSON.stringify(carritoActual),\n  pantalla_actual: 'agregar_producto',\n  ultimo_cambio: new Date().toISOString(),\n  row_number: rowNumber,\n  id_producto_agregado: id_producto\n}}];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -400,
        576
      ],
      "id": "0a2d7929-cc42-464b-ab7d-0d3dcc7af97f",
      "name": "Procesar Agregar al Carrito"
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=194098520#gid=194098520",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 194098520,
          "mode": "list",
          "cachedResultName": "SESSIONS"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -576,
        576
      ],
      "id": "ed08389e-383d-4f2e-b781-77976cc9d5bf",
      "name": "Leer Sesión Actual",
      "alwaysOutputData": true,
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "operation": "update",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=194098520#gid=194098520",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 194098520,
          "mode": "list",
          "cachedResultName": "SESSIONS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit#gid=194098520"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "telegram_id": "={{ $json.telegram_id }}",
            "nombre": "={{ $json.nombre }}",
            "pantalla_actual": "={{ $json.pantalla_actual }}",
            "carrito_temporal": "={{ $json.carrito_temporal }}",
            "ultimo_cambio": "={{ $json.ultimo_cambio }}",
            "row_number": 0
          },
          "matchingColumns": [
            "telegram_id"
          ],
          "schema": [
            {
              "id": "telegram_id",
              "displayName": "telegram_id",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "nombre",
              "displayName": "nombre",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "apellido",
              "displayName": "apellido",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "pantalla_actual",
              "displayName": "pantalla_actual",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "carrito_temporal",
              "displayName": "carrito_temporal",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "ultimo_cambio",
              "displayName": "ultimo_cambio",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "row_number",
              "displayName": "row_number",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "number",
              "canBeUsedToMatch": true,
              "readOnly": true,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        48,
        608
      ],
      "id": "9bf289fc-0709-48bc-8b7f-3018cbc2d563",
      "name": "Guardar Carrito en Sesión",
      "alwaysOutputData": true,
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=194098520#gid=194098520",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 194098520,
          "mode": "list",
          "cachedResultName": "SESSIONS"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -736,
        768
      ],
      "id": "dda4d4c3-2283-40ba-95db-7301c94eca41",
      "name": "Leer Carrito Actual",
      "alwaysOutputData": true,
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=1797496043#gid=1797496043",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1797496043,
          "mode": "list",
          "cachedResultName": "MENU"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -560,
        784
      ],
      "id": "8d2b5a1b-7969-4300-af0b-147bc9935ff8",
      "name": "Leer Todo el Menú",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const parserData = $('Parser de Mensajes').item.json;\nconst sesiones = $('Leer Carrito Actual').all();\nconst todosProductos = $('Leer Todo el Menú').all();\n\nconst sesionUsuario = sesiones.find(s => String(s.json.telegram_id) === String(parserData.telegram_id));\n\nif (!sesionUsuario || !sesionUsuario.json.carrito_temporal) {\n  return [{ json: {\n    chat_id: parserData.chat_id,\n    texto: '🧺 Tu carrito está vacío.\\n\\nExplora el menú para agregar productos.',\n    teclado: JSON.stringify({ inline_keyboard: [[{ text: '🛒 Ver Menú', callback_data: 'ver_menu' }],[{ text: '🏠 Inicio', callback_data: 'inicio' }]] })\n  }}];\n}\n\nlet carrito;\ntry { carrito = JSON.parse(sesionUsuario.json.carrito_temporal); } catch(e) { carrito = []; }\nif (!Array.isArray(carrito) || !carrito.length) {\n  return [{ json: {\n    chat_id: parserData.chat_id,\n    texto: '🧺 Tu carrito está vacío.\\n\\nExplora el menú para agregar productos.',\n    teclado: JSON.stringify({ inline_keyboard: [[{ text: '🛒 Ver Menú', callback_data: 'ver_menu' }],[{ text: '🏠 Inicio', callback_data: 'inicio' }]] })\n  }}];\n}\n\nconst menuMap = {};\ntodosProductos.forEach(p => { if (p.json.id_producto) menuMap[p.json.id_producto] = p.json; });\n\nlet total = 0;\nlet texto = '🧺 *Tu carrito:*\\n\\n';\nconst botones = [];\n\ncarrito.forEach(item => {\n  const prod = menuMap[item.id];\n  if (!prod) return;\n  const precio = Number(prod.precio);\n  const subtotal = precio * item.cantidad;\n  total += subtotal;\n  texto += `• *${prod.nombre}*\\n x${item.cantidad} · $${precio.toLocaleString('es-CO')} c/u = _$${subtotal.toLocaleString('es-CO')}_\\n\\n`;\n  botones.push([{ text: `➕ +1 ${prod.nombre}`, callback_data: `add_${item.id}`}]);\n});\n\ntexto += `━━━━━━━━━━━━━━\\n💰 _Total: $${total.toLocaleString('es-CO')}_`;\nbotones.push([{ text: '💳 Confirmar y pagar', callback_data: 'confirmar_pedido' }]);\nbotones.push([{ text: '❌ Cancelar pedido', callback_data: 'cancelar_pedido' }]);\nbotones.push([{ text: '🗑 Vaciar carrito', callback_data: 'vaciar_carrito' },{ text: '🛒 Seguir comprando', callback_data: 'ver_menu' }]);\n\nreturn [{ json: { chat_id: parserData.chat_id, texto, teclado: JSON.stringify({ inline_keyboard: botones }), total }}];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -304,
        832
      ],
      "id": "d95e55c3-7fc7-49b9-a7b6-696bc5802044",
      "name": "Calcular y Renderizar Carrito"
    },
    {
      "parameters": {
        "chatId": "={{ $json.chat_id }}",
        "text": "={{ $json.texto }}",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "🔴Cancelar pedido",
                    "additionalFields": {
                      "callback_data": "cancelar_pedido"
                    }
                  },
                  {
                    "text": "✅ Confirmar pedido - Pagar",
                    "additionalFields": {
                      "callback_data": "confirmar_pedido"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "🔙 Menú",
                    "additionalFields": {
                      "callback_data": "ver_menu"
                    }
                  },
                  {
                    "text": "🗑 Vaciar carrito",
                    "additionalFields": {
                      "callback_data": "vaciar_carrito"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "🛒 Seguir comprando",
                    "additionalFields": {
                      "callback_data": "ver_menu"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        -80,
        832
      ],
      "id": "00f8bf43-360e-47a1-8797-ac9cbcf4d997",
      "name": "Mostrar Carrito",
      "webhookId": "18c5386e-eef4-4652-a4c4-4e0bdd1d4487",
      "alwaysOutputData": true,
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "operation": "clear",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=194098520#gid=194098520",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 194098520,
          "mode": "list",
          "cachedResultName": "SESSIONS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit#gid=194098520"
        }
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -768,
        944
      ],
      "id": "a7b76a6a-9e56-4cd6-866e-0d75d4439b25",
      "name": "Vaciar Carrito en Sheets",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $json.chat_id }}",
        "text": "={{ $json.mensaje }}",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "🛒 Ver Carrito",
                    "additionalFields": {
                      "callback_data": "ver_carrito"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        304,
        1024
      ],
      "id": "6c2bb536-5167-4e5b-a01e-9255280b28fb",
      "name": "Notificar Error Pedido",
      "webhookId": "9d3b56fc-10e7-41af-b11b-fa81d2314172",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "operation": "update",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=194098520#gid=194098520",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 194098520,
          "mode": "list",
          "cachedResultName": "SESSIONS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit#gid=194098520"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "telegram_id": "={{ $('Generar Pedido2').item.json.id_usuario }}",
            "pantalla_actual": "pedido_confirmado",
            "carrito_temporal": "[]",
            "ultimo_cambio": "={{ $now }}",
            "row_number": 0,
            "nombre": "={{ $json.nombre }}"
          },
          "matchingColumns": [
            "telegram_id"
          ],
          "schema": [
            {
              "id": "telegram_id",
              "displayName": "telegram_id",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "nombre",
              "displayName": "nombre",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "pantalla_actual",
              "displayName": "pantalla_actual",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "carrito_temporal",
              "displayName": "carrito_temporal",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "ultimo_cambio",
              "displayName": "ultimo_cambio",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "row_number",
              "displayName": "row_number",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "number",
              "canBeUsedToMatch": true,
              "readOnly": true,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        1456,
        1024
      ],
      "id": "24ae020c-e353-4319-8ea0-a31da8a035ec",
      "name": "Limpiar Carrito Confirmado",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Generar Pedido2').item.json.chat_id }}",
        "text": "=🎉 *¡Pedido confirmado\\!*\n\n📋 *N° Pedido:* `{{ $('Generar Pedido2').item.json.id_pedido }}`\n👤 *Cliente:* {{ $('Generar Pedido2').item.json.nombre_cliente }}\n📅 *Fecha:* {{ $('Generar Pedido2').item.json.fecha }} \\- {{ $('Generar Pedido2').item.json.hora }}\n\n*🛒 Detalle:*\n{{ $('Generar Pedido2').item.json.detalles_texto }}\n\n━━━━━━━━━━━━━━\n💲 *Subtotal:* {{ $('Generar Pedido2').item.json.subtotal_fmt }}\n📈 *Impuesto \\(8%\\):* {{ $('Generar Pedido2').item.json.impuesto_fmt }}\n💰 *Total a pagar:* {{ $('Generar Pedido2').item.json.total_fmt }}\n⏱️ *Estado:* Recibido 📥\n\n_Te notificaremos cuando tu pedido esté listo\\._",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "📋 Ver mis pedidos",
                    "additionalFields": {
                      "callback_data": "mis_pedidos"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "🏠 Menú principal",
                    "additionalFields": {
                      "callback_data": "inicio"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        624,
        1120
      ],
      "id": "faf9189c-d73c-4d20-8630-a48882969373",
      "name": "Notificar Pedido Confirmado",
      "webhookId": "c18f4593-4b61-486a-b305-859255f4d0b2",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=1877641098#gid=1877641098",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1877641098,
          "mode": "list",
          "cachedResultName": "PEDIDOS"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -784,
        1376
      ],
      "id": "2bc02564-a215-41ad-a1bd-d9c0e9f14df0",
      "name": "Leer Mis Pedidos",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const parserData = $('Parser de Mensajes').item.json;\nconst pedidos = $input.all();\n\nconst misPedidos = pedidos.filter(p => String(p.json.id_usuario) === String(parserData.telegram_id));\n\nif (!misPedidos.length || !misPedidos[0].json.id_pedido) {\n  return [{ json: { chat_id: parserData.chat_id, texto: '📭 *No tienes pedidos registrados aún.*\\n\\nUsa el menú para hacer tu primer pedido.', teclado: JSON.stringify({ inline_keyboard: [[{ text: '🛒 Ver Menú', callback_data: 'ver_menu' }]] }) }}];\n}\n\nconst ultimos = misPedidos.slice(-5).reverse();\nconst estadoEmoji = { 'Recibido': '📥', 'Preparación': '👨‍🍳', 'En camino': '🚴', 'Entregado': '✅', 'Cancelado': '❌' };\nlet texto = '📋 *Tus últimos pedidos:*\\n\\n';\nultimos.forEach(p => {\n  const e = estadoEmoji[p.json.estado] || '📦';\n  texto += `${e} *${p.json.id_pedido}*\\n`;\n  texto += ` 📅 ${p.json.fecha} ${p.json.hora}\\n`;\n  texto += `  💰 $${Number(p.json.total_pago).toLocaleString('es-CO')}\\n`;\n  texto += ` Estado: *${p.json.estado}*\\n\\n`;\n});\n\nreturn [{ json: { chat_id: parserData.chat_id, texto, teclado: JSON.stringify({ inline_keyboard: [[{ text: '🛒 Nuevo pedido', callback_data: 'ver_menu' }],[{ text: '🏠 Inicio', callback_data: 'inicio' }]] }) }}];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -576,
        1376
      ],
      "id": "4cdfa3ad-b27f-48e7-9d4f-aa2908ec1b42",
      "name": "Formatear Historial"
    },
    {
      "parameters": {
        "chatId": "={{ $json.chat_id }}",
        "text": "={{ $json.texto }}",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": []
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        -368,
        1376
      ],
      "id": "d974fcbc-04b0-480a-a358-afd00cb4bb18",
      "name": "Mostrar Mis Pedidos",
      "webhookId": "49b75492-2bec-48f9-8ce2-8555c8c05a27",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Parser de Mensajes').item.json.chat_id }}",
        "text": "❓ *Ayuda - DeliveryBot*\n\n*Comandos disponibles:*\n• /start  → Menú principal\n\n*¿Cómo hacer un pedido?*\n1️⃣ Toca *Ver Menú* → elige categoría\n2️⃣ Agrega producto de tu eleccion disponible en las opciones\n3️⃣ Confirma tu eleccion y continua\n4️⃣ Toca *Confirmar pedido*\n5️⃣ Recibirás notificaciones de estado 🚀\n\n*Precios incluyen 8% de impuesto.*",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "🏠 Ir al inicio",
                    "additionalFields": {
                      "callback_data": "inicio"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        -896,
        1760
      ],
      "id": "1eeb6322-db53-4830-9e14-b832a2e69a6b",
      "name": "Mostrar Ayuda",
      "webhookId": "e79dd4bd-51f4-4f08-8dba-fb4d687c1d27",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 2
          },
          "conditions": [
            {
              "leftValue": "={{ $json.mensaje }}",
              "rightValue": "/start",
              "operator": {
                "type": "string",
                "operation": "equals"
              },
              "id": "7b8010a6-65d3-4545-8a3f-5b572ad46467"
            },
            {
              "leftValue": "={{ $json.mensaje }}",
              "rightValue": "inicio",
              "operator": {
                "type": "string",
                "operation": "equals"
              },
              "id": "23dcced4-8ab4-4f46-906e-72b24a3eee07"
            }
          ],
          "combinator": "or"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        -1344,
        320
      ],
      "id": "67c030ee-3271-492c-879c-f2cccf73ff13",
      "name": "¿Es /start o Inicio?1"
    },
    {
      "parameters": {
        "chatId": "={{ $('Parser de Mensajes').item.json.chat_id }}",
        "text": "🍽️ *¡Bienvenid@ a DeliveryBot Restaurante\\!*\n\nHola ¿qué deseas ordenar hoy?",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "🛒 Ver Menú",
                    "additionalFields": {
                      "callback_data": "ver_menu"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "🧺 Mi Carrito",
                    "additionalFields": {
                      "callback_data": "ver_carrito"
                    }
                  },
                  {
                    "text": "📋 Mis Pedidos",
                    "additionalFields": {
                      "callback_data": "mis_pedidos"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "❓ Ayuda",
                    "additionalFields": {
                      "callback_data": "ayuda"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        -992,
        32
      ],
      "id": "1fb23bbc-5536-48ac-acfb-e0d1f6a02b19",
      "name": "Bienvenida1",
      "webhookId": "0989dc6b-f205-451b-848f-63de8e34a8f4",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=1797496043#gid=1797496043",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1797496043,
          "mode": "list",
          "cachedResultName": "MENU"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -672,
        400
      ],
      "id": "786ad01a-1681-4940-bcf7-b04ec7c003a4",
      "name": "Leer Menú por Categoría1",
      "alwaysOutputData": true,
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Parser de Mensajes').item.json.chat_id }}",
        "text": "✅ *¡Producto agregado al carrito\\!*\n\n¿Qué deseas hacer ahora?",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "🟢 Seguir comprando",
                    "additionalFields": {
                      "callback_data": "ver_menu"
                    }
                  },
                  {
                    "text": "🔴 Ver mi carrito",
                    "additionalFields": {
                      "callback_data": "ver_carrito"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        336,
        688
      ],
      "id": "2f4db1a7-5c96-4d28-95a8-6ad13b3ff79e",
      "name": "Preguntar Quieres Algo Mas1",
      "webhookId": "6bd87df9-1cee-406c-8c96-9fb73429a10d",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=194098520#gid=194098520",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 194098520,
          "mode": "list",
          "cachedResultName": "SESSIONS"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -752,
        1152
      ],
      "id": "984555cb-2767-406c-b611-74bd73e2755f",
      "name": "Leer Sesión para Confirmar2",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=1797496043#gid=1797496043",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1797496043,
          "mode": "list",
          "cachedResultName": "MENU"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -544,
        1168
      ],
      "id": "4f2865c8-b269-42ec-b952-0637e94d5bad",
      "name": "Leer Menú para Confirmar2",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const parserData = $('Parser de Mensajes').item.json;\nconst sesiones = $('Leer Sesión para Confirmar2').all();\nconst todosProductos = $('Leer Menú para Confirmar2').all();\n\nconst sesionUsuario = sesiones.find(s => String(s.json.telegram_id) === String(parserData.telegram_id));\n\nif (!sesionUsuario || !sesionUsuario.json.carrito_temporal) {\n  return [{ json: { error: true, chat_id: parserData.chat_id, mensaje: '⚠️ Tu carrito está vacío. Agrega productos antes de confirmar.' } }];\n}\n\nlet carrito;\ntry { carrito = JSON.parse(sesionUsuario.json.carrito_temporal); } catch(e) {\n  return [{ json: { error: true, chat_id: parserData.chat_id, mensaje: '❌ Error al leer el carrito. Intenta de nuevo.' } }];\n}\nif (!Array.isArray(carrito) || !carrito.length) {\n  return [{ json: { error: true, chat_id: parserData.chat_id, mensaje: '⚠️ Tu carrito está vacío.' } }];\n}\n\nconst menuMap = {};\ntodosProductos.forEach(p => { if (p.json.id_producto) menuMap[p.json.id_producto] = p.json; });\n\nlet subtotal = 0;\nconst detallesTexto = [];\nconst detallesSimplificado = [];\nconst productosVendidos = [];\nconst productosAActualizar = [];\nconst erroresStock = [];\n\ncarrito.forEach(item => {\n  const prod = menuMap[item.id];\n  if (!prod) return;\n  const cantidad = parseInt(item.cantidad || 1);\n  const stockDisponible = parseInt(prod.stock || 999);\n  if (cantidad > stockDisponible) {\n    erroresStock.push(`• ${prod.nombre} (Pedido: ${cantidad} | Disponible: ${stockDisponible})`);\n    return;\n  }\n  const precio = Number(prod.precio);\n  const sub = precio * cantidad;\n  subtotal += sub;\n  detallesTexto.push(`• ${prod.nombre} x${cantidad} → $${sub.toLocaleString('es-CO')}`);\n  detallesSimplificado.push(`${prod.nombre} (x${cantidad})`);\n  productosVendidos.push({ id: item.id, nombre: prod.nombre, cantidad, precio });\n  productosAActualizar.push({ id_producto: item.id, nuevo_stock: stockDisponible - cantidad });\n});\n\nif (erroresStock.length > 0) {\n  return [{ json: { error: true, chat_id: parserData.chat_id, mensaje: '⚠️ *Sin stock suficiente de:*\\n\\n' + erroresStock.join('\\n') + '\\n\\nAjusta tu carrito.' } }];\n}\n\nconst impuesto = subtotal * 0.08;\nconst total_pago = subtotal + impuesto;\nconst ahora = new Date();\nconst id_pedido = `PED-${ahora.getFullYear()}${String(ahora.getMonth()+1).padStart(2,'0')}${String(ahora.getDate()).padStart(2,'0')}-${String(ahora.getTime()).slice(-5)}`;\nconst fecha = ahora.toLocaleDateString('es-CO');\nconst hora = ahora.toLocaleTimeString('es-CO', { hour: '2-digit', minute: '2-digit' });\n\nreturn [{ json: {\n  id_pedido, id_usuario: parserData.telegram_id, chat_id: parserData.chat_id,\n  nombre_cliente: parserData.nombre,\n  detalles_pedido: detallesSimplificado.join(' | '),\n  detalles_texto: detallesTexto.join('\\n'),\n  productos_json: JSON.stringify(productosVendidos),\n  subtotal_fmt: '$' + subtotal.toLocaleString('es-CO'),\n  impuesto_fmt: '$' + impuesto.toLocaleString('es-CO'),\n  total_fmt: '$' + total_pago.toLocaleString('es-CO'),\n  estado: 'Recibido', fecha, hora,\n  productos_update: productosAActualizar, error: false\n}}];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -352,
        1200
      ],
      "id": "af2c4db1-0725-47d5-babe-9dcb943e2f48",
      "name": "Generar Pedido2"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 2
          },
          "conditions": [
            {
              "leftValue": "={{ $json.error }}",
              "rightValue": true,
              "operator": {
                "type": "boolean",
                "operation": "equals"
              },
              "id": "399ed572-a982-40c8-a9c0-ad1e338d1733"
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        80,
        1136
      ],
      "id": "825ac123-2387-4ffe-8561-3092825c2224",
      "name": "¿Error en pedido?2"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=1877641098#gid=1877641098",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1877641098,
          "mode": "list",
          "cachedResultName": "PEDIDOS"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "id_pedido": "={{ $json.id_pedido }}",
            "id_usuario": "={{ $json.id_usuario }}",
            "total_pago": "={{ $json.total_fmt }}",
            "estado": "={{ $json.estado }}",
            "fecha": "={{ $json.fecha }}",
            "hora": "={{ $json.hora }}",
            "detalles_pedidos": "={{ $json.detalles_pedido }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "id_pedido",
              "displayName": "id_pedido",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "id_usuario",
              "displayName": "id_usuario",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "detalles_pedidos",
              "displayName": "detalles_pedidos",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "total_pago",
              "displayName": "total_pago",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "estado",
              "displayName": "estado",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "fecha",
              "displayName": "fecha",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "hora",
              "displayName": "hora",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        384,
        1216
      ],
      "id": "d05cd068-8a5d-485a-8c42-2dcf470eb1ff",
      "name": "Guardar Pedido en Sheets2",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Generar Pedido2').item.json.chat_id }}",
        "text": "=🍳 *¡NUEVO PEDIDO ENTRANTE\\!* 🍳\n──────────────────────\n🆔 *Orden:* {{ $('Generar Pedido2').item.json.id_pedido }}\n👤 *Cliente:* {{ $('Generar Pedido2').item.json.nombre_cliente }}\n🕒 *Hora:* {{ $('Generar Pedido2').item.json.hora }}\n\n📋 *Detalle:*\n{{ $('Generar Pedido2').item.json.detalles_texto }}\n──────────────────────\n💲 *Subtotal:* {{ $('Generar Pedido2').item.json.subtotal_fmt }}\n📈 *Impuesto (8%):* {{ $('Generar Pedido2').item.json.impuesto_fmt }}\n💵 *Total a Cobrar:* {{ $('Generar Pedido2').item.json.total_fmt }}",
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        1680,
        1040
      ],
      "id": "a5c571c0-ae13-4f65-8487-15d6db6727af",
      "name": "Notificar Cocina1",
      "webhookId": "3de1daf6-770c-4913-ab93-e2354f426859",
      "alwaysOutputData": true,
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "rule": {
          "interval": [
            {
              "field": "cronExpression",
              "expression": "0 21 * * *"
            }
          ]
        }
      },
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.2,
      "position": [
        -560,
        1904
      ],
      "id": "b68086cf-bd16-4417-9bbc-656a3046701b",
      "name": "Reporte BI Diario (9 PM)1"
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1877641098,
          "mode": "list",
          "cachedResultName": "PEDIDOS"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -336,
        1904
      ],
      "id": "d14d0d4f-f727-4117-ae7b-280be730bd9f",
      "name": "Leer Pedidos del Día1",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const pedidos = $input.all();\nconst hoy = new Date().toLocaleDateString('es-CO');\nconst pedidosHoy = pedidos.filter(p => p.json.fecha === hoy);\nif (!pedidosHoy.length) {\n  return [{ json: { total_vendido: 0, producto_estrella: 'Ninguno', hora_pico: 'N/A', cantidad_pedidos: 0, vacio: true, fecha: hoy } }];\n}\nlet totalVendido = 0;\nconst contadorProductos = {};\nconst contadorHoras = {};\npedidosHoy.forEach(p => {\n  const item = p.json;\n  totalVendido += Number(item.total_pago || 0);\n  if (item.hora) {\n    const horaLimpia = item.hora.split(':')[0] + ':00';\n    contadorHoras[horaLimpia] = (contadorHoras[horaLimpia] || 0) + 1;\n  }\n  if (item.detalles_pedido) {\n    item.detalles_pedido.split(' | ').forEach(prodStr => {\n      const parts = prodStr.split(' (x');\n      const nombre = parts[0].trim();\n      const cantidad = parts[1] ? parseInt(parts[1].replace(')', '')) : 1;\n      contadorProductos[nombre] = (contadorProductos[nombre] || 0) + cantidad;\n    });\n  }\n});\nconst productoEstrella = Object.entries(contadorProductos).sort((a,b)=>b[1]-a[1])[0] || ['N/A',0];\nconst horaPico = Object.entries(contadorHoras).sort((a,b)=>b[1]-a[1])[0] || ['N/A',0];\nreturn [{ json: { fecha: hoy, total_vendido: totalVendido, producto_estrella: `${productoEstrella[0]} (${productoEstrella[1]} uds)`, hora_pico: `${horaPico[0]} (${horaPico[1]} pedidos)`, cantidad_pedidos: pedidosHoy.length, vacio: false } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -112,
        1904
      ],
      "id": "2205ea31-d644-4fda-9eb3-88da14de73d5",
      "name": "Procesar Métricas BI1"
    },
    {
      "parameters": {
        "chatId": "={{ $env.ADMIN_CHAT_ID }}",
        "text": "📊 *REPORTE DIARIO DE INTELIGENCIA DE NEGOCIO* 📊\n📅 *Fecha:* {{ $json.fecha }}\n────────────────────────\n💰 *Total Vendido:* ${{ $json.total_vendido.toLocaleString('es-CO') }}\n📦 *Pedidos Totales:* {{ $json.cantidad_pedidos }}\n⭐ *Producto Estrella:* {{ $json.producto_estrella }}\n⏰ *Hora Pico:* {{ $json.hora_pico }}\n────────────────────────\n_Reporte generado automáticamente por DeliveryBot._",
        "additionalFields": {
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        112,
        1904
      ],
      "id": "9f6779af-b4e0-4615-a1ec-73831b897b56",
      "name": "Enviar Reporte BI1",
      "webhookId": "94200335-3e9c-44be-a796-60a2f15380f8",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "1b18867d-0ee5-475e-8b00-abb87a5463e8",
              "leftValue": "={{ $json.mensaje }}",
              "rightValue": "ver_menu",
              "operator": {
                "type": "string",
                "operation": "equals"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        -736,
        256
      ],
      "id": "737bfc98-deff-4c5e-a0a9-2a33c341c18f",
      "name": "If"
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.callback_query.message.chat.id }}",
        "text": "📋 _Selecciona una categoría de nuestro menú:_",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "☀️ Almuerzos",
                    "additionalFields": {
                      "callback_data": "cat_almuerzos"
                    }
                  },
                  {
                    "text": "🥤 Bebidas",
                    "additionalFields": {
                      "callback_data": "cat_bebidas"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "🍿 Snacks",
                    "additionalFields": {
                      "callback_data": "cat_snacks"
                    }
                  },
                  {
                    "text": "🍰 Postres",
                    "additionalFields": {
                      "callback_data": "cat_postres"
                    }
                  }
                ]
              }
            },
            {
              "row": {
                "buttons": [
                  {
                    "text": "🔙 Volver",
                    "additionalFields": {
                      "callback_data": "inicio"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false,
          "parse_mode": "Markdown"
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        -544,
        160
      ],
      "id": "abd37214-4e95-4949-8dca-280485e72e91",
      "name": "Ver menu por categoria",
      "webhookId": "8aa90700-d9ee-4f96-89b4-52351e5e4fb0",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=194098520#gid=194098520",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 194098520,
          "mode": "list",
          "cachedResultName": "SESSIONS",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit#gid=194098520"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "telegram_id": "={{ $json.telegram_id }}",
            "nombre": "={{ $json.nombre }}",
            "pantalla_actual": "={{ $json.pantalla_actual }}",
            "carrito_temporal": "={{ $json.carrito_temporal }}",
            "ultimo_cambio": "={{ $json.ultimo_cambio }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "telegram_id",
              "displayName": "telegram_id",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "nombre",
              "displayName": "nombre",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "pantalla_actual",
              "displayName": "pantalla_actual",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "carrito_temporal",
              "displayName": "carrito_temporal",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "ultimo_cambio",
              "displayName": "ultimo_cambio",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -176,
        576
      ],
      "id": "f85af4a2-b9c9-4b84-9205-e3b02af0f19d",
      "name": "Append row in sheet",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $json.chat_id }}",
        "text": "❌ _Pedido cancelado_ Tu pedido ha sido cancelado. Tu carrito fue vaciado. ¿Qué deseas hacer?",
        "replyMarkup": "inlineKeyboard",
        "inlineKeyboard": {
          "rows": [
            {
              "row": {
                "buttons": [
                  {
                    "text": "🛒 Ver Menú",
                    "additionalFields": {
                      "callback_data": "ver_menu"
                    }
                  },
                  {
                    "text": "🏠 Inicio",
                    "additionalFields": {
                      "callback_data": "inicio"
                    }
                  }
                ]
              }
            }
          ]
        },
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        -576,
        992
      ],
      "id": "91825fc1-1116-47c7-b1a2-f6fa0e12e441",
      "name": "Cancelar pedido",
      "webhookId": "20556c44-acbd-407e-9239-104364768808",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const pedidoBase = $input.first().json;\nconst productosAActualizar = pedidoBase.productos_update || [];\n\nif (!productosAActualizar.length) {\n  return [{ json: {\n    id_producto: 'NONE',\n    stock: 0,\n    chat_id: pedidoBase.chat_id,\n    telegram_id: pedidoBase.id_usuario,\n    id_pedido: pedidoBase.id_pedido,\n    sin_productos: true\n  }}];\n}\n\nreturn productosAActualizar.map(p => ({\n  json: {\n    id_producto: p.id_producto,\n    stock: p.nuevo_stock,\n    chat_id: pedidoBase.chat_id,\n    telegram_id: pedidoBase.id_usuario,\n    id_pedido: pedidoBase.id_pedido\n  }\n}));"
      },
      "id": "27ea849a-bc5d-4988-82f4-b57c22790a35",
      "name": "Preparar Actualizacion Stock",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        944,
        1056
      ]
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=1797496043#gid=1797496043",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1797496043,
          "mode": "list",
          "cachedResultName": "MENU",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit#gid=1797496043"
        },
        "filtersUI": {
          "values": [
            {
              "lookupColumn": "stock",
              "lookupValue": "50"
            },
            {
              "lookupColumn": "stock",
              "lookupValue": "30"
            },
            {
              "lookupColumn": "stock",
              "lookupValue": "25"
            },
            {
              "lookupColumn": "stock",
              "lookupValue": "20"
            },
            {
              "lookupColumn": "stock",
              "lookupValue": "15"
            },
            {
              "lookupColumn": "stock",
              "lookupValue": "18"
            },
            {}
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -128,
        1296
      ],
      "id": "7f2968b8-3d80-466e-8562-8a15eae449b1",
      "name": "Consultar stock",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "0a8f35e2-c096-4fba-b22e-90f45bf14562",
              "leftValue": "={{ $json.productos_update[0].nuevo_stock }}",
              "rightValue": 3,
              "operator": {
                "type": "number",
                "operation": "equals"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        80,
        1472
      ],
      "id": "321a13f3-a8c3-4981-a220-31a27c2b4fa1",
      "name": "Es menor a 3?"
    },
    {
      "parameters": {
        "operation": "update",
        "documentId": {
          "__rl": true,
          "value": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?pli=1&gid=1797496043#gid=1797496043",
          "mode": "url"
        },
        "sheetName": {
          "__rl": true,
          "value": 1797496043,
          "mode": "list",
          "cachedResultName": "MENU",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit#gid=1797496043"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "stock": "={{ $json.stock }}"
          },
          "matchingColumns": [
            "stock"
          ],
          "schema": [
            {
              "id": "id_producto",
              "displayName": "id_producto",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "nombre",
              "displayName": "nombre",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "descripcíon",
              "displayName": "descripcíon",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "precio",
              "displayName": "precio",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "categoria",
              "displayName": "categoria",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "stock",
              "displayName": "stock",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "row_number",
              "displayName": "row_number",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "number",
              "canBeUsedToMatch": true,
              "readOnly": true,
              "removed": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        1296,
        1024
      ],
      "id": "e4cd5e13-e2de-4f46-a62c-cacd6b1dad4d",
      "name": "Actualizar Stock final",
      "alwaysOutputData": true,
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "5006869687",
        "text": "=⚠️ ALERTA DE STOCK: El producto ´{{ $('Generar Pedido2').item.json }}´ tiene pocas unidades. Favor reabastecer.",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        352,
        1424
      ],
      "id": "7e39e343-6f43-4855-96fc-0b3f8fe3570d",
      "name": "Bajo stock",
      "webhookId": "dfa99526-6503-4c80-8e42-37c016af81fc",
      "credentials": {
        "telegramApi": {
          "id": "hlSleZV385UERok6",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU",
          "mode": "list",
          "cachedResultName": "Datos cafeteria n8n",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": 1797496043,
          "mode": "list",
          "cachedResultName": "MENU",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/135r3nRZzl1jfzCAwvIMrniDAeuD11CFgVCbH0u-evpU/edit#gid=1797496043"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        1152,
        1040
      ],
      "id": "09d1c7aa-1e3f-4ce6-971f-b9c79df7dcb4",
      "name": "Consultar stock2",
      "alwaysOutputData": true,
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "bkjQ1qfwWOkdK1hg",
          "name": "Google Sheets account"
        }
      }
    }
  ],
  "connections": {
    "Telegram Trigger": {
      "main": [
        [
          {
            "node": "Parser de Mensajes",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Parser de Mensajes": {
      "main": [
        [
          {
            "node": "¿Es /start o Inicio?1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Registrar Usuario": {
      "main": [
        [
          {
            "node": "Bienvenida1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Router Principal": {
      "main": [
        [
          {
            "node": "If",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Leer Menú por Categoría1",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Leer Sesión Actual",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Leer Carrito Actual",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Leer Sesión para Confirmar2",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Vaciar Carrito en Sheets",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Leer Mis Pedidos",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Mostrar Ayuda",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Switch Categorias": {
      "main": [
        [
          {
            "node": "Filtrar Bebidas",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Filtrar Almuerzos",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Filtrar Snacks",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Filtrar Postres",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Filtrar Bebidas": {
      "main": [
        [
          {
            "node": "Lista Bebidas",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Filtrar Almuerzos": {
      "main": [
        [
          {
            "node": "Lista Almuerzos",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Filtrar Snacks": {
      "main": [
        [
          {
            "node": "Lista Snacks",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Filtrar Postres": {
      "main": [
        [
          {
            "node": "Lista Postres",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Procesar Agregar al Carrito": {
      "main": [
        [
          {
            "node": "Append row in sheet",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Leer Sesión Actual": {
      "main": [
        [
          {
            "node": "Procesar Agregar al Carrito",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Guardar Carrito en Sesión": {
      "main": [
        [
          {
            "node": "Preguntar Quieres Algo Mas1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Leer Carrito Actual": {
      "main": [
        [
          {
            "node": "Leer Todo el Menú",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Leer Todo el Menú": {
      "main": [
        [
          {
            "node": "Calcular y Renderizar Carrito",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Calcular y Renderizar Carrito": {
      "main": [
        [
          {
            "node": "Mostrar Carrito",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Vaciar Carrito en Sheets": {
      "main": [
        [
          {
            "node": "Cancelar pedido",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Limpiar Carrito Confirmado": {
      "main": [
        [
          {
            "node": "Notificar Cocina1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Notificar Pedido Confirmado": {
      "main": [
        [
          {
            "node": "Preparar Actualizacion Stock",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Leer Mis Pedidos": {
      "main": [
        [
          {
            "node": "Formatear Historial",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Formatear Historial": {
      "main": [
        [
          {
            "node": "Mostrar Mis Pedidos",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "¿Es /start o Inicio?1": {
      "main": [
        [
          {
            "node": "Registrar Usuario",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Router Principal",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Leer Menú por Categoría1": {
      "main": [
        [
          {
            "node": "Switch Categorias",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Leer Sesión para Confirmar2": {
      "main": [
        [
          {
            "node": "Leer Menú para Confirmar2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Leer Menú para Confirmar2": {
      "main": [
        [
          {
            "node": "Generar Pedido2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Generar Pedido2": {
      "main": [
        [
          {
            "node": "¿Error en pedido?2",
            "type": "main",
            "index": 0
          },
          {
            "node": "Consultar stock",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "¿Error en pedido?2": {
      "main": [
        [
          {
            "node": "Notificar Error Pedido",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Guardar Pedido en Sheets2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Guardar Pedido en Sheets2": {
      "main": [
        [
          {
            "node": "Notificar Pedido Confirmado",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Reporte BI Diario (9 PM)1": {
      "main": [
        [
          {
            "node": "Leer Pedidos del Día1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Leer Pedidos del Día1": {
      "main": [
        [
          {
            "node": "Procesar Métricas BI1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Procesar Métricas BI1": {
      "main": [
        [
          {
            "node": "Enviar Reporte BI1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If": {
      "main": [
        [
          {
            "node": "Ver menu por categoria",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Append row in sheet": {
      "main": [
        [
          {
            "node": "Guardar Carrito en Sesión",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Preparar Actualizacion Stock": {
      "main": [
        [
          {
            "node": "Consultar stock2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Consultar stock": {
      "main": [
        [
          {
            "node": "Es menor a 3?",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Es menor a 3?": {
      "main": [
        [
          {
            "node": "Bajo stock",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Actualizar Stock final": {
      "main": [
        [
          {
            "node": "Limpiar Carrito Confirmado",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Consultar stock2": {
      "main": [
        [
          {
            "node": "Actualizar Stock final",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "pinData": {},
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "1901914c7c000a29b791c1c77b6a2b3c3174e98d95bb356f02edb6b1442b299a"
  }
}´´´´
